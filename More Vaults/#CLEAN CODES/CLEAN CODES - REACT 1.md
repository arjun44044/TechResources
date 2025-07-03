
**1. The issue might be due to `setOpenSubcategories` causing unexpected behavior when combined with asynchronous dispatch calls. React’s `setState` functions like `setOpenSubcategories` are asynchronous themselves, and any side effects (such as async `dispatch`) might be completing out of order when paired with state updates inside the loop.*
**To ensure proper ordering, you can separate the `dispatch` logic and `setOpenSubcategories` updates, or make sure `setOpenSubcategories` completes before continuing with the recursive dispatch calls.**

```jsx
const checkNestedSubcategories = async (id, isChecked) => {
  if (subCategories[id]) {
    for (const subCategory of subCategories[id]) {
      // Update open subcategories
      await new Promise((resolve) => {
        setOpenSubcategories((prevOpenSubcategories) => {
          const updatedState = {
            ...prevOpenSubcategories,
            [subCategory._id]: {
              ...prevOpenSubcategories[subCategory._id],
              checked: isChecked,
              status: prevOpenSubcategories[subCategory._id]?.status || false,
            },
          };
          resolve(updatedState); // Resolve when update completes
          return updatedState;
        });
      });

      // Wait for the dispatch to complete before proceeding to next subcategory
      await dispatch(getSingleCategory({ id: subCategory._id }));

      // Recursively check nested subcategories
      await checkNestedSubcategories(subCategory._id, isChecked);
    }
  }
};
```

2. **Promise.All and async in arr.map()**

```jsx
const compressedImageBlobs = async(images)=>{
                return await Promise.all( images.map( async(image)=> {
                    if(image.size > (5*1024*1024)){
                        const newBlob = await handleImageCompression(image.blob)
                        return newBlob
                    }else{
                        return image.blob
                    }
                }) )
            }
            
            const newBlobs = await compressedImageBlobs(images)
            
            newBlobs.forEach((blob, index) => {
                formData.append('images', blob, `productImg${index}`);

            })
```

3. **Make the dispatch await completely  unwrap + Make the timings in useEffect execution in sync or in the way you want(Use states for this)**

```jsx
import { unwrapResult } from '@reduxjs/toolkit';
.............
............
const [loadingSubCategories, setLoadingSubCategories] = useState(false);
........
useEffect(() => {
    if (populatedSubCategories && !loadingSubCategories) {
      setSubCategories((prevSubCategories) => ({
        ...prevSubCategories,
        [populatedSubCategories.parentId]: populatedSubCategories.subCategories,
      }));
      setOpenSubcategories((prevOpenSubcategories) => ({
        ...prevOpenSubcategories,
        [populatedSubCategories.parentId]: {
          ...prevOpenSubcategories[populatedSubCategories.parentId],
          parentLevelCount: populatedSubCategories.parentLevelCount,
        },
      }));
     checkNestedSubcategories(populatedSubCategories.parentId, true);
    }
  }, [populatedSubCategories, loadingSubCategories]);
  ............
  ..............
  const checkboxHandler = async (e, catId, subCategory) => {
    const categoryId = e.target.id;
    const isChecked = e.target.checked;
    setOpenSubcategories((prevOpenSubcategories) => ({
      ...prevOpenSubcategories,
      [catId]: {
        ...prevOpenSubcategories[catId],
        checked: isChecked,
      },
    }));
    if (subCategory.length > 0) {
      if (subCategories[catId]) {
        console.log("Inside if subCategories[catId]",)
        await checkNestedSubcategories(catId, isChecked);
      } else {
        console.log("Inside else subCategories[catId]")
        setLoadingSubCategories(true)
        try{
          console.log("Dispatching category inside checkboxHandler-->",categoryId)
          const result = await dispatch(getSingleCategory({ id: catId }))
          console.log(`Dispatched category inside checkboxHandler-->name-${categoryId}, id-${catId}`)
          unwrapResult(result)
          console.log("Going to checkNestedSubcategories from checkboxHandler--")
          setLoadingSubCategories(false)
        }
        catch (error) {
          console.error("Error fetching subcategory inside checkboxHandler--", error);
          setLoadingSubCategories(false)
        }
      }
    }
    setCheckedCategories((prevCategories) => {
      const updatedCategories = prevCategories.filter((cat) => cat.name !== categoryId);
      return isChecked ? [...updatedCategories, { name: categoryId, status: true }] : updatedCategories;
    });
  };
```

4. Making an ARRAY of refs DYNAMICALLY----
```jsx
export function SelectSubCategoryForAdmin({category, setCategory, setSubcategory, categoryImgPreview, setCategoryImgPreview}){

    const [error, setError] = useState('')
    const [levelOneCategories, setLevelOneCategories] = useState([])
    const [nestedSubcategories, setNestedSubcategories] = useState([])
    const [checkNestedSubcategories, setCheckNestedSubcategories] = useState({})
    const [checkedCategories, setCheckedCategories] = useState({});
    const [subcategoryLabel, setsubcategoryLabel] = useState('')
    const [defaultDisabled, setDefaultDisabled] = useState(true)
    const {categories, populatedSubCategories, firstLevelCategories} = useSelector(state=> state.categoryStore)

    const dispatch = useDispatch()
    const categoryRefs = useRef({})

    useEffect(()=>{
        dispatch(getFirstLevelCategories())
        dispatch(resetSubcategories())
        if(nestedSubcategories) setNestedSubcategories([])
    },[])
    useEffect(()=>{
        console.log("category.length------>", category.length)
        if(category.length == 0){
            setDefaultDisabled(true)
            setCheckedCategories({})
            setNestedSubcategories([])
            setCheckNestedSubcategories({})
        }else{
            setDefaultDisabled(false)
        }
    },[category])

    useEffect(()=>{
        console.log("defaultDisabled---->", defaultDisabled)
    },[defaultDisabled])

    useEffect(() => {
        console.log("firstLevelCategories from SelectSubCategoryForAdmin --->", JSON.stringify(firstLevelCategories))
        if (firstLevelCategories && firstLevelCategories.length) {
            firstLevelCategories.forEach(category => {
                if (!categoryRefs.current[category.name]) {
                    categoryRefs.current[category.name] = React.createRef();
                }
            })

            Object.values(categoryRefs.current).forEach(refs => {
                if (refs?.current) {
                    console.log("category-->",category)
                    const isActive = category.some(cat => refs.current.id.includes(cat));
                    console.log(`isActive is ${isActive} for ${refs.current.id}`)
                    if (!isActive) {
                        refs.current.style.color = '#919191';
                        refs.current.previousElementSibling.style.color = '#919191';
                        refs.current.inactivate = true;
                        setCheckedCategories(prev => ({
                            ...prev,
                            [refs.current.id]: false,
                        }));
                    } else {
                        refs.current.style.color = 'initial';
                        refs.current.previousElementSibling.style.color = '#757575';
                        refs.current.inactivate = false;
                    }
                }
            });
        } else if (!firstLevelCategories.length) {
            Object.values(categoryRefs.current).forEach(ref => {
                if (ref?.current) {
                    ref.current.style.color = '#757575';
                    ref.current.previousElementSibling.style.color = '#757575';
                    ref.current.inactivate = true;
                    console.log("Activated: ", ref.current.id)
                }
            });
        }
    }, [category]);
 }
```

5. To work with inputs of type radio, and how to control them--
	
```jsx
const radioClickHandler = (e)=>{
        const checkStatus = checkedCategories?.[e.target.parentElement.parentElement.id]?.[e.target.id]
        console.log("checkStatus-->", checkStatus)
        if(checkStatus){
            console.log("Inisde if checkStatus")
            setCheckedCategories({ ...checkedCategories, [e.target.parentElement.parentElement.id]: false})
            setNestedSubcategories([])
            setCheckNestedSubcategories({})
            setCategoryImgPreview('')
            return
        }else{
            const changeEvent = new Event('change', {bubbles:true})
            e.target.dispatchEvent(changeEvent)
        }
    }
    const radioChangeHandler = (e, subcat)=>{
        console.log("Object.values(checkedCategories)-->",Object.values(checkedCategories))
        if(!Object.values(checkedCategories).every(status=> status === false)){
            e.target.checked = false
            setError('Cannot select more than 1 subcategory!')
            console.log("Cannot select more than 1 subcategory!")
            return
        }else{
            if(subcat.subCategory){
                dispatch(getSingleCategory({id: subcat._id}))
            }
            setCheckedCategories({ ...checkedCategories, [e.target.parentElement.parentElement.id]: {...checkedCategories[e.target.parentElement.parentElement.id], [e.target.id]: e.target.checked} })
            setCategoryImgPreview(subcat.image.url)
        }
    }
```

6. Using Array.from() in a function inside a component which is called in the jsx part of the component

```jsx
export default function OtpVerificationPage(){
----------
--------
const OtpBoxGenerator = (counts, verificationError)=> {
        const inputArr = Array.from({length: counts}, (_, index) => (
            <input key={index} type="text" autoComplete="off" disabled={otpBoxDisabled} className={`w-[2.5rem] p-[10px] pl-[12px] border-[2px] 
                        ${verificationError? 'border-red-500 bg-red-200' : 'border-white'} rounded-[6px] text-secondary`} 
                            value={values[index]} onChange={(e)=>handleChange(e, index)} onBlur={(e)=>{handleBlur(e)}}
                                onKeyDown={(e) => handleKeyDown(e, index)} onClick={otpClickHandler}/>
        ))
        return inputArr
    }
    return(
        <>
        <section style={bgImg} className='h-[130vh]' id="otp-verify">
            <Header/>
            <main className='transform translate-x-[-50%] translate-y-[-50%] absolute top-[50%] left-[50%] w-[35%]
                             rounded-[22px] px-[50px] ' style={{marginBlock:'7%'}}>
                <h1 className='text-secondary font-funCity text-[27px] mb-[40px] text-center my-[50px]'>
                    <span className='font-funCity'> Verification </span></h1>
                    <div className='flex flex-col gap-[15px] text-descReg1 items-start mb-[60px]' onSubmit={(e)=>submitData(e)} >
                        <div className='w-full flex items-center justify-center gap-[5px] check-mail'>
                            <h2 className='text-center text-[17px] tracking-[1px]' style={{wordSpacing:'0.5px'}}>Check your Email</h2>
                            <span> <IoMail/> </span>
                        </div>
                        <h4 className='w-full text-center text-secondary tracking-[0.3px]' style={{wordSpacing: '0.5px'}}>
                            Enter the verification code sent to 
                            <span className={`${userEmail? 'text-primary':'text-secondary'}`}> {userEmail ? userEmail : 'your mail'} </span>
                        </h4>
                        <hr className='w-[15rem] my-[20px] self-center border border-[#6f6e6e]'></hr>
                        <div className='self-center flex items-center gap-[1rem]'>
                            {   loading ? <CustomPacmanLoader loading={loading} size={25}/>
                                        : OtpBoxGenerator(5, verificationError)
                            }
                        </div>
    ----------
    ----------
	)
}
```

`Array.from()`**

The `Array.from()` method in JavaScript is used to create a new array from:

- **An array-like object** (like `{length: X}`).
- **An iterable object** (like a `string`, `Map`, or `Set`).

Here, `Array.from()` is used to create an array of a specific length (`counts`) where each element represents an OTP input box.
SYNTAX--
Array.from(arrayLike, mapFn);

- **`arrayLike`:** An object with a `length` property (e.g., `{length: counts}`).
- **`mapFn`:** (Optional) A mapping function applied to each element of the array, just like `Array.prototype.map`.

Array.from({ length: counts }, (_, index) => ... );

- `{ length: counts }`: Creates an array-like object with `counts` number of elements.
- `(_, index)`: The mapping function. Here, `_` represents the value (which is unused), and `index` represents the current index of the element.
- 
 Parameters `{ length: counts }`**
The `{length: counts}` object is passed to `Array.from()` to generate an array of size `counts`.
- For example:
-Array.from({ length: 5 });
- This produces an array `[undefined, undefined, undefined, undefined, undefined]` of length 5. The values are `undefined` because no mapping function is applied yet.
3. Parameters `(_, index)`**
The second argument of `Array.from()` is a mapping function that runs for every element in the newly created array.
- **Parameters:**
    
    - `_`: Represents the current element's value in the array. In this case, it is `undefined` because the array is created from `{length: counts}` with no values assigned.
    - `index`: Represents the current index of the array element.
- Example:
Array.from({ length: 5 }, (_, index) => index);
[0, 1, 2, 3, 4]


7. MODAL

```jsx
return(
    <main id="message-modal">

      {modelIsOpen && (
        <div className="modal-main">
          <div className="modal-content">
            <h2 className="text-xl font-bold mb-4"> {title} </h2>
            <p className="text-gray-600 mb-6 capitalize text-[13px]"  style={{wordSpacing:'0.5px'}}>
              {content}
            </p>
            <p className="text-[12px] text-secondary tracking-[0.3px]"> 
                {instruction}
            </p>
            <input type='text' className="border border-gray-300 rounded p-2 mb-4 w-full h-[30px] text-[11px]"
                 placeholder="Type here..." value={message} onChange={(e)=> inputHandler(e)} />
            <div className="flex items-center justify-center gap-[2rem]">
                <button type='button' className="bg-primary text-white font-500 px-[2rem] py-[3px] tracking-[0.3px] rounded hover:bg-red-500" 
                            onClick={buttonHandler} > {buttonText1} </button>
                <button onClick={closeModal} className="bg-primary text-white font-500 px-[2rem] py-[3px] tracking-[0.3px] rounded hover:bg-green-500">
                  {buttonText2}
                </button>
            </div>
          </div>
        </div>
      )}
    </main>
)
```
  )
}

#message-modal .modal-main {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.5); 
  backdrop-filter: blur(5px); 
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 5; 
}
#message-modal .modal-main .modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  width: 400px;
  max-width: 90%;
  text-align: center;
}

--The `inset` property in CSS is a shorthand for setting the **`top`**, **`right`**, **`bottom`**, and **`left`** properties of an element in one declaration. It defines how far an element is positioned from its containing block, essentially acting as a shortcut to individually specifying these four directional offsets.
inset: <top> <right> <bottom> <left>;
--This sets the modal to have `0` distance from all sides of its containing block. It essentially makes the element stretch across the entire parent container, covering the viewport completely when `position: fixed` is used.

8. OBJECTS AS STATE

```

    const [activeSorter, setActiveSorter] = useState({field:'',order:''})
    const sortHandler = (e,type,order)=>{
        console.log("typeof localUsers[0][type]-->"+typeof localUsers[0][type])
        if(e.target.style.height=='15px'){
                e.target.style.height='10px'
                e.target.style.color='rgba(159, 42, 240, 0.5)'
                console.log("Going to default icon settings and localUsers--")
                setLocalUsers(allUsers)
                console.log("Localusers now-->"+JSON.stringify(localUsers))
            }
            else {
                setActiveSorter({field:type, order})
                sortUsers(type, order, false)
            }
    }
    const sortUsers = (type, order, returnData)=> {
        console.log("Sorting.....")
        console.log(`type-->${type}, order-->${order}`)
        console.log("tempUsers now inside sortUsers-->", tempUsers)
        const sortedNames = order==1? tempUsers.map(user=>user[type]).sort() 
                                        : typeof tempUsers[0][type] == "string"? tempUsers.map(user=>user[type]).sort((a,b)=>b.localeCompare(a))                                                            : tempUsers.map(user=>user[type]).sort((a,b)=>b-a)
        const sortedUsers = []
        for(let i=0; i<sortedNames.length; i++){
            for(let user in tempUsers){ 
                if(tempUsers[user][type] == sortedNames[i])
                    sortedUsers.push(tempUsers[user])
            }
        }
        console.log("SortedNames-->"+sortedNames)
        console.log("sortedUsers-->"+JSON.stringify(sortedUsers)) 
        if(returnData){
            return sortedUsers
        }else{
            setLocalUsers(sortedUsers)
        }
    }

			   <tr className='secondaryLight-box border border-[rgb(220, 230, 166)] font-[500] text-secondary table-header'>
                          <td>
                          <div className='flex items-center'>
                                    <span>Name</span>
                                    <i className='flex flex-col h-[5px]'>
                                        <FaSortUp  onClick = {(e)=>{ sortHandler(e,"username",1)}} 
                                                style={{height: activeSorter.field === "username" && activeSorter.order === 1 ?'15px':'10px',
                                                            color: activeSorter.field === "username" && activeSorter.order === 1 ? 
                                                                        'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}}/>
                                        <FaSortDown onClick = {(e)=>sortHandler(e,"username",-1)} 
                                            style={{height: activeSorter.field === "username" && activeSorter.order === -1 ?'15px':'10px',
                                                        color: activeSorter.field === "username" && activeSorter.order === -1 ? 
                                                        'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)' }}/>
                                    </i>
                                </div> 
                            </td>
                            <td>
                                <div className='flex items-center'>
                                    <span>Email</span>
                                    <i className='flex flex-col h-[5px]'>
                                        <FaSortUp onClick = {(e)=>sortHandler(e,"email",1)} 
                                            style={{height: activeSorter.field === "email" && activeSorter.order === 1 ?'15px':'10px',
                                                        color: activeSorter.field === "email" && activeSorter.order === 1 ?
                                                            'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}}/>
                                        <FaSortDown onClick = {(e)=>sortHandler(e,"email",-1)} 
                                            style={{height: activeSorter.field === "email" && activeSorter.order === -1 ?'15px':'10px',
                                                        color: activeSorter.field === "email" && activeSorter.order === -1 ? 
                                                         'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}}/>
                                    </i>
                                </div>
                            </td>
                            <td>
                                <div className='flex items-center'>
                                    <span>Mobile</span>
                                    <i className='flex flex-col h-[5px]'>
                                        <FaSortUp onClick = {(e)=>sortHandler(e,"mobile",1)} 
                                            style={{height: activeSorter.field === "mobile" && activeSorter.order === 1 ?'15px':'10px',
                                                    color: activeSorter.field === "mobile" && activeSorter.order === 1 ? 
                                                       'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}}/>
                                        <FaSortDown onClick = {(e)=>sortHandler(e,"mobile",-1)} 
                                            style={{height: activeSorter.field === "mobile" && activeSorter.order === -1 ?'15px':'10px',
                                                        color: activeSorter.field === "mobile" && activeSorter.order === -1 ? 
                                                            'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}} />
                                    </i>
                                </div>
                            </td>
                            <td>
                                <div className='flex items-center'>
                                    <span>Wallet</span>
                                    <i className='flex flex-col h-[5px]'>
                                        <FaSortUp onClick = {(e)=>sortHandler(e,"wallet",1)}
                                            style={{height: activeSorter.field === "wallet" && activeSorter.order === 1 ?'15px':'10px',
                                                        color: activeSorter.field === "wallet" && activeSorter.order === 1 ? 
                                                            'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}}/>
                                        <FaSortDown onClick = {(e)=>sortHandler(e,"wallet",-1)}
                                            style={{height: activeSorter.field === "wallet" && activeSorter.order === -1 ?'15px':'10px',
                                                         color: activeSorter.field === "wallet" && activeSorter.order === -1 ? 
                                                            'rgba(159, 42, 240, 1)' : 'rgba(159, 42, 240, 0.5)'}}/>
                                    </i>
                                </div>
                            </td>
```
```

9. TIMER

```jsx
const [deleteDelay, setDeleteDelay ] = useState(null)
    const [deleteThisId, setDeleteThisId] = useState(null)
    const [initiateDeleteHandler, setInitiateDeleteHandler] = useState(false)
    let timerId = useRef(null)

const deleteHandler = (clearTimer=false) => {
        if(deleteThisId){
            setDeleteDelay(15);
            const id = deleteThisId
            console.log("Set delete delay to 10 seconds");
            timerId.current = setInterval(() => {
                setDeleteDelay(prevCount => {
                    if (prevCount <= 1) {
                        clearInterval(timerId.current); 
                        dispatch(deleteUser(id)); 
                        setLocalUsers(prevUsers => prevUsers.filter(user => user._id !== id));
                        console.log("User deletion dispatched");
                        return null;
                    } else {
                        console.log(`Countdown: ${prevCount - 1} seconds remaining`);
                        return prevCount - 1;
                    }
                });
            }, 1000)
        }
    }
    const undoDeleteHandler = ()=>{
        if(timerId.current){
            clearInterval(timerId.current)
            setDeleteDelay(0)
            timerId.current = null
            setDeleteThisId(null)
        }
    }
```
```
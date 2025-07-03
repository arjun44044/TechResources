
1 SOMETIMES WHEN  INPUT OF TYPE CHCEKBOX WONT GET UNCHECKED COMLPLETELY, ITS COZ OF THE OF THE ABSENCE OF key ATTRIBUTE

```jsx
<input type='checkbox' id={category.name} onChange={(e)=> manualCheckboxHandler(e, category.name)} key={`${category._id}-${manualCheckCategory?.[category.name]}`}
   checked={manualCheckCategory?.[category.name]} />

const manualCheckboxHandler = (e, name)=> {
    console.log('e.target.checked ---->', e.target.checked);
    const status = e.target.checked;
    if(status){
      setManualCheckCategory(prevObj=> {
        return {...prevObj, [name]:true}
      })
    }else{
      console.log("manualCheckCategory---->", JSON.stringify(manualCheckCategory))
      setManualCheckCategory(prevObj=> {
        return {...prevObj, [name]:false}
      })
    }
  }
                                
```

````javascript
import {configureStore, combineReducers} from '@reduxjs/toolkit'

import {persistReducer,persistStore} from 'redux-persist'

import storage from 'redux-persist/lib/storage'

import sessionStorage from 'redux-persist/lib/storage/session'

import { PAUSE,PERSIST,REGISTER,REHYDRATE,PURGE,FLUSH } from 'redux-persist'

  

import userReducer from '../Slices/userSlice'

import adminReducer from '../Slices/adminSlice'

import addressReducer from '../Slices/addressSlice'

import productReducer from '../Slices/productSlice'

import categoryReducer from '../Slices/categorySlice'

import wishlistReducer from '../Slices/wishlistSlice'

import couponReducer from '../Slices/couponSlice'

import offerReducer from '../Slices/offerSlice'

import cartReducer from '../Slices/cartSlice'

import orderReducer from '../Slices/orderSlice'

import walletReducer from '../Slices/walletSlice'

  
  

const rootReducer = combineReducers({

    user: userReducer,

    admin: adminReducer,

    address: addressReducer,

    productStore: productReducer,

    categoryStore: categoryReducer,

    wishlist: wishlistReducer,

    coupons: couponReducer,

    offers: offerReducer,

    cart: cartReducer,

    order: orderReducer

})

  

const persistConfig = {

    key: 'root',

    storage,

    // blacklist: ["success", "error"]

}

  

const walletPersistConfig = {

    key: 'wallet',

    storage: sessionStorage

}

  

const persistedReducer = persistReducer(persistConfig, rootReducer);

  

const store = configureStore({

    reducer:persistedReducer,

    middleware:(getDefaultMiddleware)=>

        getDefaultMiddleware({

            serializableCheck:{

                ignoreActions:[PAUSE,PERSIST,REGISTER,REHYDRATE,PURGE,FLUSH]

            }

        })

})

export const persistor = persistStore(store)

export default store
```



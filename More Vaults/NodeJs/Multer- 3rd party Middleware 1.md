`Multer` is a middleware for handling `multipart/form-data`, which is commonly used for uploading files. When using `Multer` with Express.js, it provides several properties and methods to handle file uploads. Below are some of the important key properties provided by `Multer`:

1. **`multer.diskStorage`**: This property is used to configure the storage engine for handling uploaded files. It takes an object with two properties:
    
    - `destination`: The directory where uploaded files should be stored.
    - `filename`: A function to determine the name of the uploaded file.
    
    Example:
    
     `const storage = multer.diskStorage({`
  `destination: (req, file, cb) => {`
    `cb(null, 'uploads/'); // Specify the destination directory`
  `},`
  `filename: (req, file, cb) => {`
    `cb(null, Date.now() + '-' + file.originalname); // Specify the filename`
  `},`
`});`
`const upload = multer({ storage: storage });`

    
- **`fileFilter`**: This property is used to specify a function that filters which files are accepted for upload. It should return `true` to accept the file, or `false` to reject it.
    
```
 const fileFilter = (req, file, cb) => {
  if (file.mimetype === 'image/jpeg') {
    cb(null, true); // Accept JPEG files
  } else {
    cb(new Error('Invalid file type'), false); // Reject other file types
  }
};
const upload = multer({ fileFilter: fileFilter });

```
- **`limits`**: This property allows you to set limits on the size and number of files that can be uploaded.
    
    Example:
        
```
```
```
- const limits = {   fileSize: 1024 * 1024, // 1 MB limit per file   files: 5, // Limit the number of files to 5 }; 
- const upload = multer({ limits: limits });
```
```
```
    
- **`storage`**: This property specifies the storage engine to be used for handling uploaded files. It is typically set to `multer.diskStorage` for disk storage or `multer.memoryStorage` for in-memory storage.
    
    
```
- `const storage = multer.memoryStorage(); 
- const upload = multer({ storage: storage });`
```
    
- **`single` and `array`**: These methods are used to specify whether a single file (`single`) or multiple files (`array`) should be uploaded.
    
    
```
1. `const singleUpload = upload.single('avatar'); // Upload a single file with field name 'avatar' 
2. const multipleUpload = upload.array('photos', 3); // Upload multiple files with field name 'photos', limit to 3 files`
```
    

#NodeJs
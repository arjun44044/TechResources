Certainly! Let's create a more comprehensive example using `addMatcher` to handle actions that match specific conditions. This example will demonstrate how to use `addMatcher` to handle multiple actions with similar logic. We will also see how to use the `isAnyOf` utility to match multiple actions.

### Scenario

We will create a Redux slice for managing a list of products. In addition to fetching products, we will handle actions for adding and removing products using async thunks. We'll use `addMatcher` to handle pending, fulfilled, and rejected states for these async thunks.

### Step-by-Step Example

#### Step 1: Define the Async Thunks

First, we create async thunks for fetching, adding, and removing products.

```javascript
import { createAsyncThunk } from '@reduxjs/toolkit';

// Simulated API functions
const fakeApiFetchProducts = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        ok: true,
        data: [
          { id: 1, name: 'Product 1' },
          { id: 2, name: 'Product 2' },
        ],
      });
    }, 1000);
  });
};

const fakeApiAddProduct = (newProduct) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        ok: true,
        data: { id: 3, name: newProduct.name },
      });
    }, 1000);
  });
};

const fakeApiRemoveProduct = (productId) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ ok: true });
    }, 1000);
  });
};

// Async thunks
export const fetchProducts = createAsyncThunk('products/fetchProducts', async (_, thunkAPI) => {
  const response = await fakeApiFetchProducts();
  if (response.ok) {
    return response.data;
  } else {
    return thunkAPI.rejectWithValue(response.error);
  }
});

export const addProduct = createAsyncThunk('products/addProduct', async (newProduct, thunkAPI) => {
  const response = await fakeApiAddProduct(newProduct);
  if (response.ok) {
    return response.data;
  } else {
    return thunkAPI.rejectWithValue(response.error);
  }
});

export const removeProduct = createAsyncThunk('products/removeProduct', async (productId, thunkAPI) => {
  const response = await fakeApiRemoveProduct(productId);
  if (response.ok) {
    return productId;
  } else {
    return thunkAPI.rejectWithValue(response.error);
  }
});
```

#### Step 2: Create the Slice with `addMatcher`

Next, we create a slice that uses `addMatcher` to handle the pending, fulfilled, and rejected states for our async thunks.

```javascript
import { createSlice, isAnyOf } from '@reduxjs/toolkit';
import { fetchProducts, addProduct, removeProduct } from './productThunks';

const productSlice = createSlice({
  name: 'products',
  initialState: { items: [], status: 'idle', error: null },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addMatcher(
        isAnyOf(fetchProducts.pending, addProduct.pending, removeProduct.pending),
        (state) => {
          state.status = 'loading';
          state.error = null;
        }
      )
      .addMatcher(
        isAnyOf(fetchProducts.fulfilled, addProduct.fulfilled, removeProduct.fulfilled),
        (state, action) => {
          state.status = 'succeeded';
          if (action.type === fetchProducts.fulfilled.type) {
            state.items = action.payload;
          } else if (action.type === addProduct.fulfilled.type) {
            state.items.push(action.payload);
          } else if (action.type === removeProduct.fulfilled.type) {
            state.items = state.items.filter((item) => item.id !== action.payload);
          }
        }
      )
      .addMatcher(
        isAnyOf(fetchProducts.rejected, addProduct.rejected, removeProduct.rejected),
        (state, action) => {
          state.status = 'failed';
          state.error = action.payload;
        }
      );
  },
});

export default productSlice.reducer;
```

### Explanation of `addMatcher`

1. **Pending State Matcher**:
   ```javascript
   builder.addMatcher(
     isAnyOf(fetchProducts.pending, addProduct.pending, removeProduct.pending),
     (state) => {
       state.status = 'loading';
       state.error = null;
     }
   );
   ```
   - **Purpose**: This matcher handles the pending state for all three async thunks.
   - **Action**: Sets the `status` to `loading` and clears any previous errors.

2. **Fulfilled State Matcher**:
   ```javascript
   builder.addMatcher(
     isAnyOf(fetchProducts.fulfilled, addProduct.fulfilled, removeProduct.fulfilled),
     (state, action) => {
       state.status = 'succeeded';
       if (action.type === fetchProducts.fulfilled.type) {
         state.items = action.payload;
       } else if (action.type === addProduct.fulfilled.type) {
         state.items.push(action.payload);
       } else if (action.type === removeProduct.fulfilled.type) {
         state.items = state.items.filter((item) => item.id !== action.payload);
       }
     }
   );
   ```
   - **Purpose**: This matcher handles the fulfilled state for all three async thunks.
   - **Action**: 
     - For `fetchProducts.fulfilled`, it sets the `items` to the fetched products.
     - For `addProduct.fulfilled`, it adds the new product to the `items`.
     - For `removeProduct.fulfilled`, it removes the product from the `items`.

3. **Rejected State Matcher**:
   ```javascript
   builder.addMatcher(
     isAnyOf(fetchProducts.rejected, addProduct.rejected, removeProduct.rejected),
     (state, action) => {
       state.status = 'failed';
       state.error = action.payload;
     }
   );
   ```
   - **Purpose**: This matcher handles the rejected state for all three async thunks.
   - **Action**: Sets the `status` to `failed` and stores the error payload.

### Using the Slice in a Store

Configure the store with the `productSlice` reducer.

```javascript
import { configureStore } from '@reduxjs/toolkit';
import productReducer from './productSlice';

const store = configureStore({
  reducer: {
    products: productReducer,
  },
});

export default store;
```

### Using the Async Thunks in a Component

Here's how you might use these thunks in a React component.

```javascript
import React, { useEffect, useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { fetchProducts, addProduct, removeProduct } from './productThunks';

const ProductList = () => {
  const dispatch = useDispatch();
  const products = useSelector((state) => state.products.items);
  const status = useSelector((state) => state.products.status);
  const error = useSelector((state) => state.products.error);
  const [newProduct, setNewProduct] = useState('');

  useEffect(() => {
    dispatch(fetchProducts());
  }, [dispatch]);

  const handleAddProduct = () => {
    dispatch(addProduct({ name: newProduct }));
    setNewProduct('');
  };

  const handleRemoveProduct = (productId) => {
    dispatch(removeProduct(productId));
  };

  if (status === 'loading') {
    return <div>Loading...</div>;
  }

  if (status === 'failed') {
    return <div>Error: {error}</div>;
  }

  return (
    <div>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            {product.name} <button onClick={() => handleRemoveProduct(product.id)}>Remove</button>
          </li>
        ))}
      </ul>
      <input
        type="text"
        value={newProduct}
        onChange={(e) => setNewProduct(e.target.value)}
        placeholder="New product name"
      />
      <button onClick={handleAddProduct}>Add Product</button>
    </div>
  );
};

export default ProductList;
```

### Summary

- **Builder API**: Provides a flexible and type-safe way to handle actions in Redux slices.
- **`addMatcher`**: Allows you to handle actions that match specific conditions, useful for handling multiple actions with similar logic.
- **`isAnyOf`**: Utility function to match multiple action types, simplifying the handling of related actions.

By using `addMatcher`, you can create more concise and maintainable reducers that handle similar actions together, reducing redundancy and improving code readability.
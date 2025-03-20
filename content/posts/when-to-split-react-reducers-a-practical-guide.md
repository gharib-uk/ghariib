---
title: "When to Split React Reducers: A Practical Guide"
date: 2025-03-19
---

Managing state in React applications can quickly become complex, especially as your application grows. One of the most common questions developers face is when and how to split their reducers. Let's dive into this topic with real-world examples and practical advice.

#### The Single Reducer Pattern: When It Works and When It Doesn't

Let's start with a common scenario: an e-commerce dashboard that manages both UI state (like modal visibility, selected tabs) and data state (product inventory, orders). Initially, you might start with a single reducer:  

```
type DashboardState = {
  // UI State
  isProductModalOpen: boolean;
  selectedTab: 'inventory' | 'orders' | 'analytics';
  sidebarCollapsed: boolean;

  // Data State
  products: Product[];
  orders: Order[];
  loadingProducts: boolean;
  loadingOrders: boolean;
  error: string | null;
};

const dashboardReducer = (state: DashboardState, action: DashboardAction): DashboardState => {
  switch (action.type) {
    case 'TOGGLE_PRODUCT_MODAL':
      return { ...state, isProductModalOpen: !state.isProductModalOpen };
    case 'SET_SELECTED_TAB':
      return { ...state, selectedTab: action.payload };
    case 'FETCH_PRODUCTS_START':
      return { ...state, loadingProducts: true };
    case 'FETCH_PRODUCTS_SUCCESS':
      return { 
        ...state, 
        products: action.payload,
        loadingProducts: false,
        error: null
      };
    // ... many more cases
  }
};
```

This quickly becomes hard to manage for several specific reasons:

1. The state shape becomes increasingly complex and intertwined, making it difficult to understand which parts of the state are related to UI and which are related to data.
2. The reducer function grows larger, often exceeding 200-300 lines of code, making it harder to maintain and test.
3. Different team members working on different features might create conflicts when modifying the same reducer file.
4. Type definitions become unwieldy as you add more state properties.

#### Splitting by Concern: UI vs. Data

A better approach is to split the reducer based on concerns. Here's how we can separate UI state from data state:  

```
// uiReducer.ts
type UIState = {
  isProductModalOpen: boolean;
  selectedTab: 'inventory' | 'orders' | 'analytics';
  sidebarCollapsed: boolean;
};

const uiReducer = (state: UIState, action: UIAction): UIState => {
  switch (action.type) {
    case 'TOGGLE_PRODUCT_MODAL':
      return { ...state, isProductModalOpen: !state.isProductModalOpen };
    case 'SET_SELECTED_TAB':
      return { ...state, selectedTab: action.payload };
    case 'TOGGLE_SIDEBAR':
      return { ...state, sidebarCollapsed: !state.sidebarCollapsed };
    default:
      return state;
  }
};

// dataReducer.ts
type DataState = {
  products: Product[];
  orders: Order[];
  loadingProducts: boolean;
  loadingOrders: boolean;
  error: string | null;
};

const dataReducer = (state: DataState, action: DataAction): DataState => {
  switch (action.type) {
    case 'FETCH_PRODUCTS_START':
      return { ...state, loadingProducts: true };
    case 'FETCH_PRODUCTS_SUCCESS':
      return {
        ...state,
        products: action.payload,
        loadingProducts: false,
        error: null
      };
    case 'FETCH_PRODUCTS_ERROR':
      return {
        ...state,
        loadingProducts: false,
        error: action.payload
      };
    // ... other data-related cases
    default:
      return state;
  }
};
```

#### When to Split Further: Domain-Driven Separation

As your application grows, you might find that even the data reducer becomes too large. This is particularly true when you're dealing with multiple domains in your application. Let's consider when you should split the data reducer further:

1. When different teams are working on different features
2. When state updates in one domain rarely affect other domains
3. When testing becomes complicated due to the size of the reducer
4. When you notice performance issues due to frequent updates

Here's an example of splitting the data reducer by domain:  

```
// productReducer.ts
type ProductState = {
  items: Product[];
  loading: boolean;
  error: string | null;
  categories: Category[];
};

const productReducer = (state: ProductState, action: ProductAction): ProductState => {
  switch (action.type) {
    case 'FETCH_PRODUCTS_START':
      return { ...state, loading: true };
    case 'UPDATE_PRODUCT_INVENTORY':
      return {
        ...state,
        items: state.items.map(product =>
          product.id === action.payload.id
            ? { ...product, inventory: action.payload.inventory }
            : product
        )
      };
    // ... other product-specific cases
  }
};

// orderReducer.ts
type OrderState = {
  orders: Order[];
  loading: boolean;
  error: string | null;
  activeFilters: OrderFilter[];
};

const orderReducer = (state: OrderState, action: OrderAction): OrderState => {
  switch (action.type) {
    case 'UPDATE_ORDER_STATUS':
      return {
        ...state,
        orders: state.orders.map(order =>
          order.id === action.payload.orderId
            ? { ...order, status: action.payload.status }
            : order
        )
      };
    // ... other order-specific cases
  }
};
```

#### Real-World Benefits of Splitting Reducers

1. **Improved Performance**: When you split reducers, React can optimize re-renders better. For example, if you update the UI state (like toggling a modal), components that only depend on order data won't re-render.
    
2. **Better Testing**: Smaller, focused reducers are easier to test. You can write specific tests for UI behavior without worrying about data state:  
    

```
describe('uiReducer', () => {
  it('should toggle product modal', () => {
    const initialState = { isProductModalOpen: false, selectedTab: 'inventory', sidebarCollapsed: false };
    const action = { type: 'TOGGLE_PRODUCT_MODAL' as const };

    const newState = uiReducer(initialState, action);
    expect(newState.isProductModalOpen).toBe(true);
  });
});
```

1. **Easier Maintenance**: When bugs occur, it's easier to isolate the problem to a specific reducer. If there's an issue with order status updates, you know exactly where to look.
    
2. **Better Type Safety**: TypeScript types become more focused and manageable:  
    

```
type ProductAction =
  | { type: 'FETCH_PRODUCTS_START' }
  | { type: 'FETCH_PRODUCTS_SUCCESS'; payload: Product[] }
  | { type: 'UPDATE_PRODUCT_INVENTORY'; payload: { id: string; inventory: number } };

// Instead of one large union type for all actions
type DashboardAction = ProductAction | OrderAction | UIAction; // This becomes unwieldy
```

#### When Not to Split Reducers

While splitting reducers has many benefits, there are cases where it might not be necessary:

1. In small applications with limited state
2. When state updates frequently need to coordinate between different domains
3. When the overhead of managing multiple reducers outweighs the benefits

The key is to find the right balance for your specific use case. Start with a single reducer, and split when you notice:

- The reducer file exceeding 200 lines
- Multiple developers frequently conflicting in the same file
- Testing becoming difficult
- Performance issues related to unnecessary re-renders

Remember, the goal of splitting reducers is to make your application more maintainable, performant, and easier to understand. Don't split just for the sake of splitting – let your application's needs guide your decision.

Go to Source

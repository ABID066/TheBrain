### Installation 

- `npm install @reduxjs/toolkit`
- `npm install react-redux`


### Create store.js

```js
import {configureStore} from "@reduxjs/toolkit";  
import settingsReducer from "../state-slice/setting-slice.js"  
import taskReducer from "../state-slice/task-slice.js";  
import summaryReducer from "../state-slice/summary-slice.js";  
import profileReducer from "../state-slice/profile-slice.js";  
  
export default configureStore({  
    reducer:{  
        settings: settingsReducer,  
        task: taskReducer,  
        summary: summaryReducer,  
        profile: profileReducer  
    }  
});
```
- Then we need to connect the store.js file with `main.jsx`


### Connecting with main.jsx

```js
import React from 'react'  
import ReactDOM from 'react-dom/client'  
import App from './App.jsx'  
import "./assets/css/bootstrap.css"  
import "./assets/css/style.css"  
import "./assets/css/amimate.min.css"  
import {Toaster} from "react-hot-toast";  
import {Provider} from "react-redux";  
import store from "./redux/store/store.js";  
  
  
ReactDOM.createRoot(document.getElementById('root')).render(  
  <React.StrictMode>  
      <Provider store={store}>  
          <App />          
          <Toaster/>      
        </Provider>  
    </React.StrictMode>,  
)
```
- In this code, we need to import `{Provider}` & `store` to connect with the project
- And the `store.js` is connected with all redux `state-slice` file


### State-Slice Files


#### task-slice.js File

```js
import {createSlice} from "@reduxjs/toolkit";  
  
export const taskSlice = createSlice({  
    name: "task",  
    initialState: {  
        New:[],  
        Completed:[],  
        Progress:[],  
        Canceled:[]  
    },  
    reducers: {  
        SetNewTask: (state, action) => {  
            state.New = action.payload;  
        },  
        SetCompletedTask: (state, action) => {  
            state.Completed = action.payload;  
        },  
        SetProgressTask: (state, action) => {  
            state.Progress = action.payload;  
        },  
        SetCanceledTask: (state, action) => {  
            state.Canceled = action.payload;  
        }  
    }  
})  
export const {  
    SetNewTask,       //Use to store the value
    SetCanceledTask,  //Use to store the value
    SetProgressTask,  //Use to store the value
	SetCompletedTask  //Use to store the value
    } = taskSlice.actions;  
export default taskSlice.reducer;
```
- In here, we store the value and get those value or `initialState` value.


#### To Store the value

```js
import {SetCanceledTask, SetCompletedTask, SetNewTask, SetProgressTask} from "../redux/state-slice/task-slice.js";
import { HideLoader, ShowLoader } from "../redux/state-slice/setting-slice.js";

import store from "../redux/store/store.js";


export async function TaskListByStatus(Status) {  
    store.dispatch(ShowLoader());  //also used elsewhere
    
    let URL = BaseURL + "/show-tasks/"+Status;  
    
    try {  
        let res = await axios.get(URL, AxiHeader());  
        if(res.status === 200) {  
            if(Status==="New"){  
                store.dispatch(SetNewTask(res.data["message"]))  //using here
            }  
            else if(Status==="Completed"){  
                store.dispatch(SetCompletedTask(res.data["message"])); //using here
            }  
            else if(Status==="In Progress"){  
                store.dispatch(SetProgressTask(res.data["message"]));  //using here
            }  
            else if (Status==="Canceled"){  
                store.dispatch(SetCanceledTask(res.data["message"]));  //using here
            }  
        } else {  
            ErrorToast("Something Went Wrong");  
        }  
    } catch (err) {  
        ErrorToast("Something Went Wrong");  
    } finally {  
        store.dispatch(HideLoader());  //also used elsewhere
    }  
}
```
- In here, to store the value into the `state-slice` we nee to use the `store.dispatch(SetProgressTask(res.data["message"]));`
- And also we need to import `store`.
- We can also use `useDispatch()` in some cases.

```js
import { useDispatch } from "react-redux";
import { SetNewTask } from "../redux/state-slice/task-slice.js";

const MyComponent = () => {
    const dispatch = useDispatch();
    
    const handleTaskUpdate = (data) => {
        dispatch(SetNewTask(data));
    };

    return <button onClick={() => handleTaskUpdate([])}>Update Task</button>;
};
```


#### To get those value

```js
import {useSelector} from "react-redux";


const InProgressList = useSelector((state)=>state.task.Progress)

const CanceledList = useSelector((state)=>state.task.Canceled)

const NewList = useSelector((state)=>state.task.New)
```
- In here, to get those value we need to use `useSelector` and import it.


### File Structure 

src/
│── APIRequest/
│── assets/
│── components/
│── helper/
│── pages/
│── redux/
│   ├── state-slice/
│   │   ├── profile-slice.js
│   │   ├── setting-slice.js
│   │   ├── summary-slice.js
│   │   ├── task-slice.js
│   ├── store/
│   │   ├── store.js
│── App.jsx
│── main.jsx


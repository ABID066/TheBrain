```js
//Home.jsx
import React, {useReducer} from 'react';
import {initialTasks} from "./data/TaskData.js";
import AddTask from "./components/AddTask.jsx";
import TaskList from "./components/TaskList.jsx";
import taskReducer from "./reducers/taskReducer"; 

const Home = () => {

    const [tasks, dispatch] = useReducer(taskReducer, initialTasks)
    
    const getNextId= (data) => {
        const maxID = data.reduce((max, task) => {
            return task.id > max ? task.id : max;
        }, 0);
        return maxID + 1;
    }
  
    const handleAddTask = (text) => {
        dispatch({
            type: "addTask",
            text,
            id: getNextId(tasks),
        })
    }

    const handleChangeTask = (task) => {
        dispatch({
            type: "changeTask",
            task,
        })
    }
  
    const handleDeleteTask = (taskId) => {
        dispatch({
            type: "deleteTask",
            id: taskId,
        })
    }  

    return (
        <div>
            <h1>Prequel Itinerary</h1>
            
            <AddTask onAdd = {handleAddTask}/>
            
            <TaskList
                tasks={tasks}
                onChangeTask={handleChangeTask}
                onDeleteTask={handleDeleteTask}
            />
        </div>
    );
};
  
export default Home;

```


```js
//taskReducer.js
export default function reducer(tasks, action)

  switch (action.type) {

        case "addTask" : {
            return [
                ...tasks,
                {
                    id: action.id,
                    text: action.text,
                    done: false,
                }
            ]
        }

        case "changeTask" : {
            return tasks.map(t=> {
                if(t.id === action.task.id){
                    return action.task;
                } else {
                    return t;
                }
            })
        }

        case "deleteTask" : {
            return tasks.filter(task => task.id !== action.id)
        }

        default: {
            throw new Error("Unknown action type " + action.type);
        }
    }

}

```
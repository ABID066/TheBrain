

```js
//App.jsx

import React from 'react';
import Heading from "./components/Heading.jsx";
import Section from "./components/Section.jsx";


const App = () => {
    return (
        <Section>
            <Heading>Title</Heading>
            
            <Section>
                <Heading>Heading</Heading>
                <Heading>Heading</Heading>
                <Heading>Heading</Heading>
                
                <Section>
                    <Heading>Sub-heading</Heading>
                    <Heading>Sub-heading</Heading>
                    <Heading>Sub-heading</Heading>
                    
                    <Section>
                        <Heading>Sub-sub-heading</Heading>
                        <Heading>Sub-sub-heading</Heading>
                        <Heading>Sub-sub-heading</Heading>
                    </Section>
                </Section>
            </Section>
        </Section>
    );
}; 

export default App;

```



```js
//Section.jsx

import { useContext } from 'react';
import { LevelContext } from "../contexts/LevelContext.jsx";

const Section = ({children}) => {

    const level = useContext(LevelContext);  //contextAPI called
    
    return (
        <div>
            <section className="section">
                <LevelContext.Provider value={level+1}>  //contextAPI used
                    {children}
                </LevelContext.Provider>
            </section>
        </div>

    );

};


export default Section;
```



```js
//Heading.jsx

import { useContext } from "react";
import { LevelContext } from "../contexts/LevelContext.jsx";

export default function Heading({ children }) {
  
    const level = useContext(LevelContext);   
  
    switch (level) {  //Heading.jsx called Section.jsx for the value of level
        case 1:
            return <h1>{children}</h1>;
        case 2:
            return <h2>{children}</h2>;
        case 3:
            return <h3>{children}</h3>;
        case 4:
            return <h4>{children}</h4>;
        case 5:
            return <h5>{children}</h5>;
        case 6:
            return <h6>{children}</h6>;
        default:
            throw Error('Unknown level: ' + level);
    }
}
```


```js
//LevelContext.jsx

import {createContext} from "react";

export const LevelContext = createContext(0)  //ContextAPI initialized and set value 0

```



![[Pasted image 20250823080155.png]]
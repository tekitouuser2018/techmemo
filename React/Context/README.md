### createContext + useState + Typescript でのオブジェクトの取り扱いについての例

```tsx
import React, { useState, createContext } from "react"

export let SampleContext = createContext({
  person: {
    height: "",
    weight: "",
  },
  handleSetPerson: (key: string, value: string) => {},
})

const [person, setPerson] = useState({
  height: "",
  weight: "",
})
const handleSetPerson = (type: string, value: string) =>
  setPerson((prevState) => ({ ...prevState, [type]: value }))
  
<SampleContext.Provider value={{ person, handleSetPerson }}>
<SampleContext.Provider>
```

参照：<br/>
https://amateur-engineer.com/react-usestate-object-update/<br/>
https://qiita.com/akifumii/items/de04e6acd761cd6dac8c

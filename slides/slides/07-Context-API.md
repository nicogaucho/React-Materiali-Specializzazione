---
revealOptions:
  transition: 'fade'
---

---

<img src="https://aulab.it/img/logo-aulab-horizontal-white.png">
<h3 class="r-fit-text">REACT MASTERCLASS</h3>

---

# Context API

---

## Obiettivi del modulo

* Passare dati attraverso componenti <!-- .element: class="fragment" -->
* Creare una instanza dati da condividere tra i componenenti <!-- .element: class="fragment" -->
* Definire useContext hook <!-- .element: class="fragment" -->
* Definire un custom hook <!-- .element: class="fragment" -->

---

## Creare un context

Dobbiamo definire un AppContext in modo da avere i dati a disposizione in ogni componente senza uso di props 

----

### ``` createContext() ```

* Set Initial value <!-- .element: class="fragment" -->
* Esporta il createContext <!-- .element: class="fragment" -->



----

### ``` AppContext.Provider ```

* Richiamare il componenti dove vogliamo condividere il context <!-- .element: class="fragment" -->
* Assicurarci richiamare un Provider <!-- .element: class="fragment" -->
* Set Initial value come props del provider <!-- .element: class="fragment" -->

----

## Esempio

```js 

export default function App() {
  const [data, setData] = useState([]);
  
  return (
    <AppContext.Provider value={{ data }}>
      <App />
    </AppContext.Provider>
  )

}

```

---

## Hook ``` useContext ```

----

Richiamare l'hook ```useContext()``` nei componenti dove usare i dati 


----

### Esempio 

```js
import { useContext } from "react";
import AppContext from "../contexts/AppContext";

export default function HomePage() {
  const appContext = useContext(AppContext);
  return (
    <>
      <h1>Home Page</h1>
      <ul>
        {appContext.data.map((el) => (
          <li key={el.id}>{el.title}</li>
        ))}
      </ul>
    </>
  )
}

```

---

## Custom Hook

----

### Esempio creazione custom Hook

```js

function useFetch(url, defaultData) {
  const [data, setData] = useState(defaultData);
  useEffect(() => {
    (async function fetchData() {
      const response = await fetch(url);
      const json = await response.json();
      setData(json);
    })()
  }, [url]);

  return {
    data
  };
}

```

----

### uso custom hook nel componente

```js

export default function App() {
  const { data } = useFetch('https://path/api/v1/data', []);

  return (
    <div>
      {data.map((data) => 
        <Data key={data.id} data={data}/>
      )}
    </div>
  );
}

```


---




## Obiettivi modulo recap

* Passare dati attraverso componenti 
* Creare una instanza dati da condividere tra i componenenti
* Definire useContext hook 
* Definire un custom hook
# Stateful Component


## Pengantar Stateful Component

Kita sudah mempelajari tentang data (props) yang bisa dikirim dari parent ke child komponen.
Disini props hanyalah bersifat read-only, atau immutable.

UI dari sebuah aplikasi kebanyakan bersifat dinamis sesuai dengan interaksi pengguna.

Data yang diolah di dalam komponen yang bersifat mutable disebut state.
State ini digunakan untuk menyimpan data yang dinamis dan sesuai dengan
interaksi user.

## Class komponen

React komponen pada materi sebelumnya biasa dibuat melalui sebuah fungsi. Ternyata selain dibuat dengan fungsi,  React komponen bisa dibuat menggunakan sintak class.

Komponen React yang dibuat menggunakan fungsi lebih dikenal dengan functional component. Sedangkan React komponen yang dibuat menggunakan __class__ disebut __class component__.

Berikut perbedaan functional komponent (di materi basic React sebelumnya sering kita sebut React komponen) dan class komponen.

functiona komponen
```jsx

import React from 'react' ;

function MyComponent({name}){
    return (
        <>
        <p>Halo, {name} !</p>
        </>
    );
}

```

class komponen
```jsx

import React from 'react' ;

class MyComponent extends React.Component {
    render(){
        const {name} = this.props;

        return (
            <>
            <p>haloo, {name}</>
            </>
        );
    }
}

```

Perbedaan mendasar dari class komponen dan Functional komponen adalah benefit yang didapatkan. Jika membuat komponen dengan class, React secara default menawarkan fitur state dan lifecylce. Karena hal inilah class komponen juga bisa disebut sebagai __stateful__ komponen.

Sedangkan functional komponen disebut __stateless__ komponen.

Meskipun stateful komponen punya banyak keuntungan, tidak disarankan untuk membuatnya secara berlebihan. Hal ini dikarenakan pembuatan komponen ini membutuhkan lebih banyak sintaks/ kode.

nb : praktik class komponen merupakan praktik lama (legacy React ?) sebelum akhirnya React memperkenalkan React Hooks di versi 16.8. Hal ini membuat standar baru functional komponen menjadi stateful dengan bantuan hooks.

### Latihan membuat class Komponen

1. codesandbox jelek banget, kita akan testing class komponen di Bolt dan stackblitz saja.

2. Buat class komponen MyKomponen pada berkas index.js lalu render dengan DOM.

index.js
```javascript

import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import MyKomponen from './MyKomponen.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <MyKomponen name='sikonto7' />
  </StrictMode>,
)

```

MyKomponen.jsx
```jsx

import React from 'react'
import './App.css'

class MyKomponen extends React.Component {
  render() {
    return (
      <p>halo bro {this.props.name}..</p>
    )
  }
}

export default MyKomponen

```
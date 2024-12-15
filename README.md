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

Karena class komponen tetaplah sebuah class, kita dapat memanfaatkan __constructor__ untuk memberikan data contohnya --state atau menulis kode yang
akan dipanggil setiap pembuatan komponen.

MyKomponen.jsx
```jsx

import React from 'react'
import './App.css'

class MyKomponen extends React.Component {
    constructor(props){
        super(props);
        console.log('komponen dibuat !');
    }

  render() {
    return (
      <p>halo bro {this.props.name}..</p>
    )
  }
}

export default MyKomponen

```

Tampilan

![Hasil render](/images/pic0001.png)

Ketika memanfaatkan __constructor__ , pastikan selalu memanggil base constructor (super) dan berikan nilai pada props.

Jika tidak, nilai this.props pada komponen akan bernilai undefined. 

Sekarang kita buat beberapa komponen dan props berbeda. Jika dicek di console. akan muncul output "Komponen dibuat !"

index.js
```javascript

import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import MyKomponen from './MyKomponen.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <MyKomponen name='sikonto7' />
    <MyKomponen name='sikonto6' />
    <MyKomponen name='sikonto5' />
  </StrictMode>,
)

```

Hasilnya sebagai berikut :

Tampilan

![Hasil render dengan props constructor](/images/pic0001b.png)

![output console](/images/pic0001c.png)

## State Komponen

Data merupakan hal yang membuat React komponen menjadi dinamis.

Data yang berada di komponen dipicu oleh interaksi dan inputan dari user. Contoh interaksi antara lain menekan tombol atau menekan tuts keyboard.

Di dalam react komponen, kita dapat menyimpan sebuah data yang bila berubah bisa memicu reaksi pada tampilan.Syaratnya, data tersebut disimpan pada state.

Meskipun props dan __state__ dapat menyimpan data, namun dua hal tersebut berbeda. Data di dalam props berasaal dari luar komponen dan diharapkan statis.
Sedangkan data pada __state__ perlu dibuat dalam komponen tersebut dan boleh diubah-ubah.

State di dalam class komponen dapat diakses lewat properti this.state, yang sebelumnya dibuat dengan constructor.

Contoh kode :

```jsx

import React from 'react' ;

class Counter extends React.Component {
    constructor(props){
        super(props);

        this.state = {
            count: 0
        };
    }
    render(){
        return (
            <>
            <p>{this.state.count}</p>
            </>
        )
    }
}

```

Perubahan data yang disimpan datam this.state akan memicu pemanggilan render() pada class komponen, itulah kunci mengapa UI selalu menampilkan data terbaru.

Namun untuk mencapai tujuan tersebut, data di state harus diubah lewat fungsi this.setState(), bukan melalui properti this.state

contoh benar
```jsx

this.setState((previousState)=>{
    return {
        count: previousState.count +1;
    }
});

```

contoh salah
```jsx

this.state.count = this.state.count +1;

```

Mari kita bahas fungsi setState() lebih dalam.

this.setState() atau setState merupakan fungsi yang khusus untuk ubah nilai state di dalam class komponen. Fungsi inilah yang sebenarnya picu pemanggilan fungsi render() ketika data di dalam state berubah.
Fungsi setState() dapat menerima dua tipe params, yaitu ojek dan fungsi yang mengembalikan objek.

parameter adalah objek
```jsx

this.setState({
    count: 0;
});
```

parameter adalah fungsi
```jsx

this.setState((previousState)=>{
    return {
        count: previousState.count +1
    }
});
```

Nilai yang diberikan objek atau yang dikembalikan fungsi, akan menjadi nilai __state__ yang baru untuk menggantikan objek state lama. namun dalam memperbarui state, fungsi this.setState() menggunakan teknik "menggabungkan", bukan mengganti keseluruhan nilai state.

Anda bisa memberikan params berupa objek ketika ingin menetapkan nilai state tanpa memperhatikan nilai state yang ada sebelumnya.

Seperti pada kasus counter, pengguna dapat implementasikan ketika ingin reset nilai dari data __count__. ketika mereset, kita tidak perlu tahu nilai sebelumnya dan cukup kita ubah ke 0.

Sedangkan untuk param fungsi, penggunaan ini cocok ketika ingin menetapkan state yang bergantung pada state sebelumnya. Misal ketika kita ingin menambah +1 ke data counter. Tentu perlu mengetahui nilai sebelumnya. Kita dapat memanfaatkan previousSate untuk hal ini.

Walaupun cara pertama terlihat singkat, tetapi secara personal lebih menyukai cara kedua dalam memanggil fungsi setState(). Hal ini dikarenakan cara kedua dapat ditetapkan pada kasus manapun. Jika kita tidak perlu nilai previousState, tinggal tidak perlu kita tuliskan saja.

Lalu, kapan dan di mana kita harus panggil fungsi setState() untuk perbarui __state__ ? Adala ketika terjadi interaksi dari pengguna. 

Untuk menangkap interaksi pengguna pada React komponen, kita perlu belajar terlebih dahulu Event Handling bekerja di React.
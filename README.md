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

function komponen
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

parameter berupa objek
```jsx

this.setState({
    count: 0;
});
```

parameter berupa fungsi
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

## Event Handling

Sejatinya, event handling di React memiliki kemiripan dengan apa yang dilakukan di DOM.
Contohnya, kita dapat menetapkan event pada elemen dengan cara seperti ini.

```html

<button onclick='increaseValue()'> + Value </button>

```

Sedangkan di React, kita menetapkan event seperti berikut .

```html

<button onClick={increaseValue}> + Value </button>

```

Pemberian nama event menggunakan CamelCase di React. Sehingga penulisannya menjadi __onClick__ .
Daftar prop selengkapnya di [sini](https://react.dev/reference/react-dom/components/common#common)

Ketika menggunakan React, anda tidak perlu menggunakan perintah addEventListener(), cukup sediakan event handler
di properti.

### Props Drilling Event handler

State sebaiknya berada di komponen parent dan hanya diubah oleh komponen itu sendiri. Maka dari
itu fungsi handler untuk mengubah state, harus berada di komponen Parent.

Ketika anda memecah komponen jadi kecil, kemungkinan anda akan menemui kasus di mana komponen
yang menerima input pengguna (tombol) dibuat secara terpisah dari komponen parent.
Ketika itu terjadi, anda perlu memberikan event handler lewat props secara driling.
Hal ini sangat umum dilakukan karena React menganut konsep __unidirectional data flow__

ilustrasi state

![state flow](/images/pic0002.png)

State __count__ hidup di dalam komponen CounterApp yang merupakan parent komponen. Karena CounterApp
pemilik state, maka dia yang berhak memperbarui data melalui fungsi onIncreaseEventHandler.

Bagan di atas menunjukkan bahwa kita memecah UI berdasarkan tugas.
Tugas pertama menampilkan data __count__ didelegasikan pada komponen CounterScreen sehingga komponen
tersebut diberikan data __count__ oleh parent melalui props.

Selanjutnya, tugas kedua yaitu menerima input utk menambah nilai count, yang didelegasikan ke komponen
IncreaseButton. Disini praktek props drilling event handler dilakukan. IncreaseButton tidak berhak mengubah
data count. Itulah sebabnya ia diberikan fungsi handler milik parent lewat props.
Fungsi handler digunakan ketika IncreaseButton menerima input dari pengguna.

Data yang disimpan di dalam state bersifat reactive. Jika data berubah, ia akan merender ulang
komponen yang ditampilkan sehingga data yang tampak akan selalu up to date.

## Latihan Komponen State dan Event Handling

Kita akan membuat aplikasi counter dengan tujuan sbb:

- tahu cara menyimpan data di __state__
- tahu cara ubah data di __state__
- memanfaatkan data di state untuk tampilkan UI
- tahu cara event handling di komponen

Aplikasi yang akan dibuat adalah counter FizzBuzz. Logika aplikasi ini adalah :  
a. bila kelipatan 5 - menampilkan "FIZZ"  
b. bila kelipatan 7 - menampilkan "BUZZ"   
c. bila kelipatan 5 dan 7 - menampikan "FIZZBUZZ"

Aplikasi diatas juga memiliki tombol "+" utk meningkatkan nilai, dan tombol reset utk mengatur
ulang.

Berikut struktur file dan folder dari aplikasi tersebut :

![struktur file](/images/pic0003.png)

File main.jsx merupakan entry point dari FizzBuzz counter. Main.jsx terdiri dari 2 komponen, yaitu komponen Title dan App.

Sedangkan komponen App terdiri dari 3 komponen yang lebih kecil, yaitu :
- CounterDisplay
- IncreaseButton
- ResetButton

Berikut beberapa kode komponen-komponennya.

CounterDisplay.jsx
```jsx

import React from 'react';

function CounterDisplay({ count }) {
  if (count === 0){
    return <p className='count'>{count}</p>
  }

  if (count %5 === 0 && count %7 === 0){
    return <p className='fizzbuzz'>FizzBuzz</p>
  }

  if (count %5 === 0){
    return <p className='fizz'>FIZZ</p>
  }

  if (count %7 === 0){
    return <p className='buzz'>BUZZ</p>
  }
  
  return (
    <div className="counter">
      <p className="count">{count}</p>
    </div>
  );
}

export default CounterDisplay;

```

IncreaseButton.jsx
```jsx

import React from 'react';

function IncreaseButton({ increase }) {
  return (
    <button onClick={increase} className="button increase">
      + Increase
    </button>
  );
}

export default IncreaseButton;

```

ResetButton.jsx
```jsx

import React from 'react';

function ResetButton({ reset }) {
  return (
    <button onClick={reset} className="button reset">
      Reset
    </button>
  );
}

export default ResetButton;

```

App.jsx
```jsx

import React, { Component } from 'react';
import './App.css';
import IncreaseButton from './IncreaseButton';
import CounterDisplay from './CounterDisplay';
import ResetButton from './ResetButton';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      count: 0
    };

    // Binding methods in constructor
    this.onIncreaseEventHandler = this.onIncreaseEventHandler.bind(this);
    this.onResetEventHandler = this.onResetEventHandler.bind(this);
  }

  onIncreaseEventHandler() {
    this.setState((previousState) => {
      return {
        count: previousState.count + 1
      };
    });
  }

  onResetEventHandler() {
    this.setState({
      count: 0
    });
  }
  
  render() {
    return (
      <>
      <div className="container">
        <CounterDisplay count={this.state.count} />
      </div>
      <div className="buttons">
          <IncreaseButton increase={this.onIncreaseEventHandler} />
          <ResetButton reset={this.onResetEventHandler} />
        </div>
        </>
    );
  }
}

export default App;

```
main.jsx
```jsx

import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
import Title from './Title.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <Title />
    <App />
  </StrictMode>,
)
```








![FizzBuzz Counter](/images/pic0004.png)   
Tampilan Counter

## Latihan Studi Kasus: Menambahkan fitur hapus kontak

Sejauh ini kita berhasil membuat class komponen dan menggunakan state. Sekarang saatnya menerapkan
hal ini ke projek sebelumnya. Kita akan menambah fitur baru. Fitur yang dimaksud adalah hapus kontak.

Kita akan menambah komponen DeleteButton.jsx

DeleteButton.jsx
```jsx

    import React from 'react';
     
    function DeleteButton({ id, onDelete }) {
      return <button className='contact-item__delete' onClick={() => onDelete(id)}>X</button>
    }
     
    export default DeleteButton;

```

Komponen DeleteButton menerima 2 properti yaitu id dan onDelete.
Properti __id__ untuk referensi id yang akan dihapus.
Sedangkan onDelete merupakan handler untuk hapus kontak.

Terlihat bahwa event onClick memanggil handler onDelete. Pemanggilan fungsi onDeete dengan diberikan nilai id
dan dibungkus anonymous function. Inilah cara memberikan nilai argumen pada fungsi event handler. selengkapnya
tentang passing argumen ke event handler di tautan [berikut](https://legacy.reactjs.org/docs/handling-events.html#passing-arguments-to-event-handlers)

nb : mengapa terdapat "on" pada onDelete ? Penamaan ini digunakan untuk menghindari reserved word di javascript.
Benar, delete adalah operator di javascript. Maka dari itu ditambahkan on untuk mengakali hal tersebut.

Selanjutnya letakkan komponen DeleteButton di dalam komponen KonakItem.

KonakItem.jsx
```jsx

import React from "react";
import ItemBody from "./ItemBody";
import ItemImg from "./ItemImg";
import DeleteButton from "./DeleteButton" ;

export default function KonakItem({imageUrl, name, tag,id,onDelete}){ // menambah props id dan onDelete
    return (
        <div className="contact-item">
            <ItemImg imageUrl={imageUrl} />
            <ItemBody name={name} tag={tag} />
            <DeleteButton id={id} onDelete={onDelete} /> <!--  menambah komponen DeleteButton -->
        </div>
    );
}

```
Selanjutnya, tombol delete akan muncul di aplikasi namun belum bisa digunakan. Hal ini karena kita belum menetapkan nilai
event handlernya.

Sekarang, kita akan memberikan nilai id dan onDelete pada komponen pembungkus dari KonakItem, yaitu KonakList.
Sesuaikan kodenya menjadi sebagai berikut.

KonakList
```jsx

import React from "react";
import KonakItem from "./KonakItem";

export default function KonakList({kontaks,onDelete}){ // tambah props onDelete
    return (
        <div className="contact-list">
           {
            kontaks.map((kontak) => (
                <KonakItem key={kontak.id} id={kontak.id} onDelete={onDelete} {...kontak} /> <!-- tambah onDelete, id-->
            ))
           }
        </div>
    );
}
```

Untuk properti onDelete, kita masih melakukan drilling karena nilai handler ada pada komponen KonakApp selaku pemilik data Kontaks.

Selanjutnya, kita akan ubah komponen KonakApp menjadi class komponen. jangan lupa saat ini kita perlu simpan data Kontaks
di __this.state__ agar perubahan datanya memicu render UI.
Kemudian buat juga method onDeleteEventHandler untuk menangani event ketika tombol hapus diklik.

Sesuaikan kode di komponen KonakApp menjadi seperti berikut :

KonakApp.jsx
```jsx

import React from "react";
import KonakList from "./KonakList";
import {getData} from './data';

/* -- kode awal ---
export default function KonakApp(){
    const kontaks = getData();

    return (
        <div className="container">
            <div className="content">
                <h1 className="title">Daftar Kontak</h1>
                <KonakList kontaks={kontaks}/>
            </div>
        </div>
    );
}
*/

class KonakApp extends React.Component{
    constructor(props){
        super(props);
        this.state={
            kontaks: getData()

        }
        this.onDeleteHandler = this.onDeleteHandler.bind(this);
    }

    onDeleteHandler(id){
        const kontaks = this.state.kontaks.filter(kontak => kontak.id !== id);
        this.setState({kontaks});
    }

    render(){

        return (
            <div className="container">
            <div className="content">
                <h1 className="title">Daftar Kontak</h1>
                <KonakList kontaks={this.state.kontaks} onDelete={this.onDeleteHandler} />
            </div>
        </div>
    );
}
}

export default KonakApp;
```

Jika kode benar, akan menghasilkan tampilan berikut :

Tampilan dengan tombol delete X  
![UI dengan delete button](/images/pic0005.png)


## Controlled Komponen (form)

Ketika anda membuat form pada aplikasi web. biasanya state atau data input berada di dalam DOM.
Di React, state sangat efektif bila dilakukan di dalam komponen. Maka dari itu jika mengelola nilai form
sebaiknya tidak dilakukan di dalam DOM, melainkan di dalam komponen dengan memanfaatkan state.

Controlled komponen merupakan komponen yang me-render form,  tetapi sumber datanya diambil dari komponen state,
bukan DOM. Alasan mengapa disebut dengan controlled komponen karena React mengontrol data form.

Contoh disini kita memiliki komponen yang me-render form dengan satu input.

```jsx

    import React from 'react';
     
    class NameForm extends React.Component {
      constructor(props) {
        super(props);
     
        this.state = {
          email: ''
        };
      }
     
      render() {
        return (
          <form>
            <input type="text" value={this.state.email} />
          </form>
        );
      }
    }
```

Pada elemen input, kita memberikan properti value dengan nilai yang berasal dari state( email).
Maka, nilai input akan selalu sama dengan nilai state __email__.
Karena itu, satu satunya cara meng-update nilai input adalah memperbarui nilai komponen state.
ini adalah contoh dari controlled element, dimana React akan mengontrol nilai dari input form.

nb : nilai input akan selalu mengikuti state, tidak terpengaruh inputan yang kita lakukan ke elemen input teks.
keanehan ini sudah diperingatkan di dokumentasi React [berikut](https://react.dev/reference/react-dom/components/input#controlling-an-input-with-a-state-variable)

Untuk mengubah nilai state email, kita perlu buat handler seperti ini :

```jsx

    import React from 'react';
     
    class NameForm extends React.Component {
      constructor(props) {
        super(props);
     
        this.state = {
          email: ''
        };
     
        this.onEmailChangeHandler = this.onEmailChangeHandler.bind(this);
      }
     
      onEmailChangeHandler(event) {
        this.setState(() => {
          return {
            email: event.target.value
          };
        });
      }
     
      render() {
        return (
          <form>
            <input 
            type="text"
            value={this.state.email}
            onChange={this.onEmailChangeHandler} />
          </form>
        );
      }
    }

```

Kapan pun input berubah, fungsi handler akan dipanggil karena kita menetapkan fungsi tersebut pada
properti onChange.

walaupun dalam membuat controlled komponen, banyak kode yang perlu ditulis, itu memiliki kelebihannya juga.
Salah satunya kita dapat menerapkan validasi secara instan. Hal ini karena state dikelola oleh komponen. Makanya
kita bisa menuliskan logika validasi di komponen dan validasi akan dijalankan setiap kali nilai state berubah.

Benefit dari hal tersebut karena adanya input dari pengguna.

Jika ada input ( perubahan) maka UI akan diperbarui menurut state terbaru. Ini adalah konsep inti React.
tidak hanya pada controlled komponen, tetapi juga komponen React secara umum.

Hal ini dinamakan [one-way data flow](https://react.dev/learn/thinking-in-react#step-2-build-a-static-version-in-react) atau [unidirectional data flow](https://legacy.reactjs.org/docs/state-and-lifecycle.html#the-data-flows-down)

## Latihan membuat Controlled Komponen

Kita akan membuat dan mengelolah nilai pada form menggunakan React. Tujuannya agar kita memahami
cara kerja controlled komponen di mana state pada form diatur oleh React, bukan DOM.

Berikut tampilan form yang akan dibuat.

![Form menggunakan React](/images/pic0006.png)

Langkah pertama adalah buatlah komponen baru MyForm.jsx lalu isikan kodenya seperti berikut:

MyForm.jsx
```jsx

import React from 'react' ;

class MyForm extends React.Component {
  render(){
    return (
      <>
      <h1>Register Form</h1>
      <form action="">
        <label htmlFor="name">Nama :</label>
        <input type="text" id="name" />
        <br />
        <label htmlFor="gender">Jenis kelamin :</label>
        <select name="gender" id="gender">
          <option value="man">Laki-laki</option>
          <option value="woman">Perempuan</option>
        </select>
        <br />
        <button type="submit">Send</button>
      </form>
      </>
    )
  }
}

export default MyForm;

```

Selanjutnya, kita bisa menambahkan komponen tersebut ke dalam index.jsx dengan membuat fragmen.
Dengan adanya fragment, kita bisa memuat Komponen KonakApp dan MyForm.

index.jsx
```jsx

import { createRoot } from 'react-dom/client'
import './index.css'
import KonakApp from './KonakApp';
import MyForm from './MyForm';


createRoot(document.getElementById('root')).render(<>
<KonakApp />
<MyForm />
</>
)
```

Hasilnya kurang lebih akan seperti ini.

![hasil MyForm](/images/pic0007.png)

Saat ini, kita bisa berinteraksi dengan form tersebut. namun state dari MyForm belum dikontrol
sepenuhnya oleh React karena data ini masih disimpan di dalam DOM. Untuk bacaan lebih dalam, silahkan
menuju link [lama (uncontrolled comps)](https://legacy.reactjs.org/docs/uncontrolled-components.html)  atau
yang [baru (controlled uncontrolled comps)](https://react.dev/learn/sharing-state-between-components#controlled-and-uncontrolled-components).

Agar nilai state dari form dapat dikontrol oleh React, kita perlu menyiapkan nilai state pada masing-masing
input yang ada.
silahkan initialize __state__ untuk nilai nama, email dan gender pada konstruktor.

MyForm
```jsx

import React from 'react' ;

class MyForm extends React.Component {
  constructor (props){ // buat state
    super(props);
    this.state = {
      name: '',
      email: '',
      gender: 'Laki-laki'
    }
  }
  render(){
    return (
      <>
      <h1>Register Form</h1>
      <form action="">
        <label htmlFor="name">Nama :</label>
        <input type="text" id="name" value={this.state.name}/> <!-- letakkan state di masing2 tempat--->
        <br />
        <label htmlFor="email">Email:</label>
        <input id="email" type="text" value={this.state.email} />
        <br />
        <label htmlFor="gender">Jenis kelamin :</label>
        <select name="gender" id="gender" value={this.state.gender}>
          <option value="man">Laki-laki</option>
          <option value="woman">Perempuan</option>
        </select>
        <br />
        <button type="submit">Send</button>
      </form>
      </>
    )
  }
}

export default MyForm;

```

Selanjutnya kita akan mengubah nilai komponen state nya.
Siapkan event handler untuk menangani perubahan state di masing-masing input.

MyForm

```jsx
import React from 'react' ;

class MyForm extends React.Component {
  constructor (props){
    super(props);
    this.state = {
      name: '',
      email: '',
      gender: 'Laki-laki'
    };
    // binding this context to event handler

    this.onNameChangeEventHandler = this.onNameChangeEventHandler.bind(this);
    this.onEmailChangeEventHandler = this.onEmailChangeEventHandler.bind(this);
    this.onGenderChangeEventHandler = this.onGenderChangeEventHandler.bind(this);

    onNameChangeEventHandler(event){
      this.setState(()=> {
        return {
          name: event.target.value
        };
      });
    }

    onEmailChangeEventHandler(event){
      this.setState(()=> {
        return {
          email: event.target.value
        };
      });
    }

    onGenderChangeEventHandler(event){
      this.setState((prevState) => {
        return {
          gender: event.target.value
        };
      });
    }

  }
  render(){
    return (
      <>
      <h1>Register Form</h1>
      <form action="">
        <label htmlFor="name">Nama :</label>
        <input type="text" id="name" value={this.state.name}/>
        <br />
        <label htmlFor="email">Email:</label>
        <input id="email" type="text" value={this.state.email} />
        <br />
        <label htmlFor="gender">Jenis kelamin :</label>
        <select name="gender" id="gender" value={this.state.gender}>
          <option value="man">Laki-laki</option>
          <option value="woman">Perempuan</option>
        </select>
        <br />
        <button type="submit">Send</button>
      </form>
      </>
    )
  }
}

export default MyForm;

```

Selanjutnya, gunakan event handler pada masing-masing element input lewat props onChange.
Fungsi event handler akan dijalankan dan mengubah nilai state setiap kali ada perubahan data
yang dilakukan oleh pengguna di input dan select.

MyForm

```jsx

import React from 'react' ;

class MyForm extends React.Component {
  constructor (props){
    super(props);
    this.state = {
      name: '',
      email: '',
      gender: 'Laki-laki'
    };
    // binding this context to event handler

    this.onNameChangeEventHandler = this.onNameChangeEventHandler.bind(this);
    this.onEmailChangeEventHandler = this.onEmailChangeEventHandler.bind(this);
    this.onGenderChangeEventHandler = this.onGenderChangeEventHandler.bind(this);

    onNameChangeEventHandler(event){
      this.setState(()=> {
        return {
          name: event.target.value
        };
      });
    }

    onEmailChangeEventHandler(event){
      this.setState(()=> {
        return {
          email: event.target.value
        };
      });
    }

    onGenderChangeEventHandler(event){
      this.setState((prevState) => {
        return {
          gender: event.target.value
        };
      });
    }

  }
  render(){
    return (
      <>
      <h1>Register Form</h1>
      <form action="">
        <label htmlFor="name">Nama :</label>
        <input type="text" id="name" value={this.state.name} onChange={this.onNameChangeEventHandler} />
        <br />
        <label htmlFor="email">Email:</label>
        <input id="email" type="text" value={this.state.email} onChange={this.onEmailChangeEventHandler}  />
        <br />
        <label htmlFor="gender">Jenis kelamin :</label>
        <select name="gender" id="gender" value={this.state.gender} onChange={this.onGenderChangeEventHandler} />
          <option value="man">Laki-laki</option>
          <option value="woman">Perempuan</option>
        </select>
        <br />
        <button type="submit">Send</button>
      </form>
      </>
    )
  }
}

export default MyForm;
```

Selanjutnya kita akan mengaktifkan alert sesuai data yang dimasukkan ke dalam elemen <input>.

MyForm
```jsx

import React from 'react' ;

class MyForm extends React.Component {
  constructor (props){
    super(props);
    this.state = {
      name: '',
      email: '',
      gender: 'Laki-laki'
    };
    // binding this context to event handler

    this.onNameChangeEventHandler = this.onNameChangeEventHandler.bind(this);
    this.onEmailChangeEventHandler = this.onEmailChangeEventHandler.bind(this);
    this.onGenderChangeEventHandler = this.onGenderChangeEventHandler.bind(this);
    this.onSubmitEventHandler = this.onEmailChangeEventHandler.bind(this);

    onNameChangeEventHandler(event){
      this.setState(()=> {
        return {
          name: event.target.value
        };
      });
    }

    onEmailChangeEventHandler(event){
      this.setState(()=> {
        return {
          email: event.target.value
        };
      });
    }

    onGenderChangeEventHandler(event){
      this.setState((prevState) => {
        return {
          gender: event.target.value
        };
      });
    }

    onSubmitEventHandler(event){
      event.preventDefault();
      const message= `
      Nama: ${this.state.name}
      Email: ${this.state.email}
      Jenis Kelamin: ${this.state.gender}
      `;

      alert(message);
    }


  }
  render(){
    return (
      <>
      <h1>Register Form</h1>
      <form action="">
        <label htmlFor="name">Nama :</label>
        <input type="text" id="name" value={this.state.name} onChange={this.onNameChangeEventHandler} />
        <br />
        <label htmlFor="email">Email:</label>
        <input id="email" type="text" value={this.state.email} onChange={this.onEmailChangeEventHandler}  />
        <br />
        <label htmlFor="gender">Jenis kelamin :</label>
        <select name="gender" id="gender" value={this.state.gender} onChange={this.onGenderChangeEventHandler} />
          <option value="man">Laki-laki</option>
          <option value="woman">Perempuan</option>
        </select>
        <br />
        <button type="submit">Send</button>
      </form>
      </>
    )
  }
}

export default MyForm;

```

Lalu terapkan event handler di element <form> lewat props onSubmit.

MyForm

```jsx

import React from 'react' ;

class MyForm extends React.Component {
  constructor (props){
    super(props);
    this.state = {
      name: '',
      email: '',
      gender: 'Laki-laki'
    };
    // binding this context to event handler

    this.onNameChangeEventHandler = this.onNameChangeEventHandler.bind(this);
    this.onEmailChangeEventHandler = this.onEmailChangeEventHandler.bind(this);
    this.onGenderChangeEventHandler = this.onGenderChangeEventHandler.bind(this);
    this.onSubmitEventHandler = this.onEmailChangeEventHandler.bind(this);

    onNameChangeEventHandler(event){
      this.setState(()=> {
        return {
          name: event.target.value
        };
      });
    }

    onEmailChangeEventHandler(event){
      this.setState(()=> {
        return {
          email: event.target.value
        };
      });
    }

    onGenderChangeEventHandler(event){
      this.setState((prevState) => {
        return {
          gender: event.target.value
        };
      });
    }

    onSubmitEventHandler(event){
      event.preventDefault();
      const message= `
      Nama: ${this.state.name}
      Email: ${this.state.email}
      Jenis Kelamin: ${this.state.gender}
      `;

      alert(message);
    }


  }
  render(){
    return (
      <>
      <h1>Register Form</h1>
      <form action="" onSubmit={this.onSubmitEventHandler}>
        <label htmlFor="name">Nama :</label>
        <input type="text" id="name" value={this.state.name} onChange={this.onNameChangeEventHandler} />
        <br />
        <label htmlFor="email">Email:</label>
        <input id="email" type="text" value={this.state.email} onChange={this.onEmailChangeEventHandler}  />
        <br />
        <label htmlFor="gender">Jenis kelamin :</label>
        <select name="gender" id="gender" value={this.state.gender} onChange={this.onGenderChangeEventHandler} />
          <option value="man">Laki-laki</option>
          <option value="woman">Perempuan</option>
        </select>
        <br />
        <button type="submit">Send</button>
      </form>
      </>
    )
  }
}

export default MyForm;

```

nb : dikarenakan dijalankan dengan bantuan stackblitz, maka ada beberapa perbaikan

Perbaikan MyForm

```jsx

import React from 'react';

class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: '',
      email: '',
      gender: 'Laki-laki'
    };

    this.onNameChangeEventHandler = this.onNameChangeEventHandler.bind(this);
    this.onEmailChangeEventHandler = this.onEmailChangeEventHandler.bind(this);
    this.onGenderChangeEventHandler = this.onGenderChangeEventHandler.bind(this);
    this.onSubmitEventHandler = this.onSubmitEventHandler.bind(this);
  }

  onNameChangeEventHandler(event) {
    this.setState(() => {
      return {
        name: event.target.value
      };
    });
  }

  onEmailChangeEventHandler(event) {
    this.setState(() => {
      return {
        email: event.target.value
      };
    });
  }

  onGenderChangeEventHandler(event) {
    this.setState(() => {
      return {
        gender: event.target.value
      };
    });
  }

  onSubmitEventHandler(event) {
    event.preventDefault();
    const message = `
      Nama: ${this.state.name}
      Email: ${this.state.email}
      Jenis Kelamin: ${this.state.gender}
    `;
    alert(message);
  }

  render() {
    return (
      <div className="container">
        <div className="content">
          <h1 className="title">Register Form</h1>
          <form onSubmit={this.onSubmitEventHandler} className="space-y-4">
            <div>
              <label htmlFor="name" className="block text-sm font-medium text-gray-700">Nama:</label>
              <input
                type="text"
                id="name"
                value={this.state.name}
                onChange={this.onNameChangeEventHandler}
                className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200"
              />
            </div>
            <div>
              <label htmlFor="email" className="block text-sm font-medium text-gray-700">Email:</label>
              <input
                id="email"
                type="email"
                value={this.state.email}
                onChange={this.onEmailChangeEventHandler}
                className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200"
              />
            </div>
            <div>
              <label htmlFor="gender" className="block text-sm font-medium text-gray-700">Jenis kelamin:</label>
              <select
                name="gender"
                id="gender"
                value={this.state.gender}
                onChange={this.onGenderChangeEventHandler}
                className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200"
              >
                <option value="Laki-laki">Laki-laki</option>
                <option value="Perempuan">Perempuan</option>
              </select>
            </div>
            <button
              type="submit"
              className="w-full bg-indigo-600 text-white py-2 px-4 rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
            >
              Submit
            </button>
          </form>
        </div>
      </div>
    );
  }
}

export default MyForm;

```
Perbaikan di KonakList

```jsx

import React from "react";
import KonakItem from "./KonakItem";

export default function KonakList({ kontaks = [], onDelete }) {
    if (!kontaks.length) {
        return (
            <div className="contact-list">
                <p className="text-gray-500 text-center">Tidak ada kontak tersedia</p>
            </div>
        );
    }

    return (
        <div className="contact-list">
            {kontaks.map((kontak) => (
                <KonakItem 
                    key={kontak.id} 
                    id={kontak.id} 
                    onDelete={onDelete} 
                    {...kontak} 
                />
            ))}
        </div>
    );
}


```

Berikut beberapa materi tentang controlled component :
- [React form](https://legacy.reactjs.org/docs/forms.html)
- [sharing state between komponent](https://react.dev/learn/sharing-state-between-components)

## Studi Kasus menambah Fitur Kontak

Kita akan menggunakan projek KonakApp 

Langkah pembuatan KonakApp dengan fitur tambah kontak :

- gunakan projek sebelumnya
- buatlah komponen KonakInput
- buat class komp KonakInput, termasuk state dan event handlernya untuk masing-masing
input (termasuk tombol tambah)

KonakInput
```jsx

import React from 'react';

class KonakInput extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      name: '',
      tag: ''
    }

    this.onNameChangeEventHandler = this.onNameChangeEventHandler.bind(this);
    this.onTagChangeEventHandler = this.onTagChangeEventHandler.bind(this);
    this.onSubmitEventHandler = this.onSubmitEventHandler.bind(this);
  }

  onNameChangeEventHandler(event) {
    this.setState(() => ({
      name: event.target.value
    }));
  }

  onTagChangeEventHandler(event) {
    this.setState(() => ({
      tag: event.target.value
    }));
  }

  onSubmitEventHandler(event) {
    event.preventDefault();
    this.props.addContact(this.state);
  }

  render() {
    return (
      <form className='contact-input' onSubmit={this.onSubmitEventHandler}>
        <input 
          type="text" 
          placeholder='nama' 
          value={this.state.name} 
          onChange={this.onNameChangeEventHandler}
        />
        <input 
          type="text" 
          placeholder='tag' 
          value={this.state.tag} 
          onChange={this.onTagChangeEventHandler}
        />
        <button type='submit'>Tambah</button>
      </form>
    )
  }
}

export default KonakInput;

```


- selanjutnya adalah menambahkan handler ke KonakApp menggunakan komponen KonakInput.
jangan lupa untuk menambahkan handler untuk tambah kontak dengan mengubah state kontaks. Berikan properti id timestamp agar
selalu unik. Tak lupa untuk menambah KonakInput ke fungsi render serta mengisikan props
dan nilainya.

KonakApp
```jsx

import React from "react";
import KonakList from "./KonakList";
import { getData } from './data';
import KonakInput from "./KonakInput";

class KonakApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      kontaks: getData(),
    }

    this.onDeleteHandler = this.onDeleteHandler.bind(this);
    this.onAddKonakHandler = this.onAddKonakHandler.bind(this);
  }

  onDeleteHandler(id) {
    const kontaks = this.state.kontaks.filter(kontak => kontak.id !== id);
    this.setState({ kontaks });
  }

  onAddKonakHandler({ name, tag }) {
    this.setState((prevState) => ({
      kontaks: [
        ...prevState.kontaks,
        {
          id: +new Date(),
          name,
          tag,
          imageUrl: '/images/default.jpeg'
        }
      ]
    }));
  }

  render() {
    return (
      <div className="container">
        <div className="content">
          <h1 className="title">Daftar Kontak</h1>
          <h2>Tambah Kontak</h2>
          <KonakInput addContact={this.onAddKonakHandler} />
          <KonakList kontaks={this.state.kontaks} onDelete={this.onDeleteHandler} />
        </div>
      </div>
    );
  }
}

export default KonakApp;

```

Jika tepat, hasilnya akan seperti dibawah ini :

![Hasil Final Studi kasus fitur kontak](/images/pic0009.png)

## Debugging komponen menggunakan DevTools

Membangun aplikasi React kadang sulit terutama jika memiliki banyak komponen.
Terutama jika melibatkan banyak props drilling, nested komponen atau adanya state yang dinamis.

Debugging aplikasi web biasanya menggunakan browser devtools. Namun sayangnya tools ini
tdak bisa menginspeksi React komponen. Maka dari itu, React DevTools hadir untuk memudahkan
kita memeriksa hierarki komponen lengkap dengan nilai pros dan state di dalamnya.

React Dev Tools tersedia dalam bentuk addon baik di firefox maupun di chrome.

Setelah memasang, setiap kali membuka aplikasi React, maka akan muncul opsi __Components & Profiler__
pada browser devtools.

Berikut tangkapan layar opsi React DevTools :

![React Devtools](/images/pic0010.png)

jangan lupa untuk mengunjungi dokumentasi resmi React DevTools [disini](https://github.com/facebook/react/tree/main/packages/react-devtools-extensions)

## Tips Penggunaan JSX untuk Nubie

Secara umum, sintaks JSX mirip HTML. Namun ada beberapa hal yang perlu diperhatikan yaitu :

- Variabel di JSX

Kapan pun menggunakan JS expression di JSX, selalu bungkus dengan { ... }

```jsx

    render() {
      const name = 'Dicoding';
     
      return (
        <div>
          <h1>Hello, {name}</h1>
          <p>Today is {new Date().toLocaleDateString()}</p>
          <p>What is 2 + 2? {2 + 2}</p>
        </div>
      )
    }

```

- Tidak ingin tampilkan apapun

Jika tidak ingin komponen menampilkan apapun, cukup return fungsi atau method __render__ dengan __null__

```jsx

    render() {
      if (isLoading()) {
        return null;
      }
      return (
        ...
      );
    }

```

- Kondisional rendering

Kemampuan menampilkan UI berdasar state merupakan fondasi frontend FW apapun.
Biasanya masing2 FW menyediakan caranya sendiri.

Jika Angular, vue ataupun svelte memiliki pendekatan berbeda, justru React hanya mendasarkan
pada standar javascript misal if else.

Berikut contoh kondisional rendering React.

```jsx

    render() {
      const authed = isAuthed()
     
      if (authed) {
        return <h1>Welcome back!</h1>
      } else {
        return <h1>Login to see your dashboard</h1>
      }
    }

```
atau bahkan dapat menggunakan ternary

```jsx

    render() {
      return isAuthed()
        ? <h1>Welcome back!</h1>
        : <h1>Login to see your dashboard</h1>
    }

```

Bahkan dapat menuliskan ekspressi di dalam kondisional rendering

```jsx

    render() {
      return (
        <div>
          <Logo />
          {isAuthed() === true
            ? <h1>Welcome back!</h1>
            : <h1>Login to see your dashboard</h1>}
        </div>
      )
    }

```

Bandingkan dengan kode Angular dan Vue berikut

Angular
```javascript

    <h1 *ngIf="authed; else elseBlock">Welcome back!</h1>
    <ng-template #elseBlock>
      <h1>Login to see your dashboard</h1>
    </ng-template>

```

Vue
```javascript

    <h1 v-if="authed">Welcome back!</h1>
    <h1 v-else>Login to see your dashboard</h1>

```

- React Fragments

Harus diingat bahwa JSX hanya mengembalikan 1 top level element (root),sehingga jika
jika ingin mengembalikan 2 atau lebih top level elemen berbeda, perlu dibungkus dengan div atau
jika tidak ingin ribet memikirkan div, dapat menggunakan React.Fragments

```jsx

    render() {
      return (
        <React.Fragment>
          <h1>Hello, Dicoding</h1>
          <p>Today is a great day!</p>
        </React.Fragment>
      )
    }

    // atau cara pintasnya

        render() {
      return (
        <> <!-- ini adalah shortcut fragment React-->
          <h1>Hello, Dicoding</h1>
          <p>Today is a great day!</p>
        </>
      )
    }

```
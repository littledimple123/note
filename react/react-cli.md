## 使用脚手架搭建react

####1 搭建脚手架

cmd中执行命令

```javascript
npx create-react-app my-app   //使用react脚手架搭建名为 my-app的react开发环境
cd my-app 
npm run start   //运行
```

#### 2 react父组件向子组件传递

父组件

```javascript
import React, { Component } from 'react'
import Record from './record'
class App extends Component {
    constructor() {
        super();
        this.state = {
                error: null,
                isLoaded: false,
                records: [{
                        'id': 1,
                        'date': '2019.1.16',
                        'title': '收入',
                        'amount': 20
                    },
                    {
                        'id': 2,
                        'date': '2019.1.15',
                        'title': '理财收益',
                        'amount': 18
                    },
                    {
                        'id': 3,
                        'date': '2019.1.14',
                        'title': '支出',
                        'amount': 10
                    }

                ]
            }
            
    }
    render() {
            return ( 
                < div >
                	<h1> Records </h1>
                	<table className = 'table table-bordered'>
                		<thead>
                			<tr>
                				<td > Date </td> 
                				<td> Title </td> 
                				<td > Amount </td>  
                			</tr> 
                		</thead> 

                		<tbody> 
                    		{this.state.records.map((record) => < Record key = { record.id } 							{...record}/>)} 
        //ES6中父组件通过解构{...record}传给子组件，子组件通过this.props.date来获取值		
        				</tbody >

                      </table >

                 </div>
               )
            }
         }

      export default App
```

```javascript
import React, { Component } from 'react'
import Record from './record'
class App extends Component {
    constructor() {
        super();
        this.state = {
                error: null,
                isLoaded: false,
                records: [{
                        'id': 1,
                        'date': '2019.1.16',
                        'title': '收入',
                        'amount': 20
                    },
                    {
                        'id': 2,
                        'date': '2019.1.15',
                        'title': '理财收益',
                        'amount': 18
                    },
                    {
                        'id': 3,
                        'date': '2019.1.14',
                        'title': '支出',
                        'amount': 10
                    }

                ]
            }
            
    }
    render() {
            return ( 
                < div >
                	<h1> Records </h1>
                	<table className = 'table table-bordered'>
                		<thead>
                			<tr>
                				<td > Date </td> 
                				<td> Title </td> 
                				<td > Amount </td>  
                			</tr> 
                		</thead> 

                		<tbody> 
                    		{this.state.records.map((record) => < Record key = { record.id } 							record={record}/>)} 	
        //ES5中父组件record属性传给子组件，子组件通过this.props.record.date来获取值			
        				</tbody >

                      </table >

                 </div>
               )
            }
         }

      export default App
```

#### 3 react中使用jquery请求api

请求成功消息:response

请求失败消息：error.responseText

```javascript
import React, { Component } from 'react'
import Record from './record'
import { getJSON } from 'jquery'
class App extends Component {
    constructor() {
        super();
        this.state = {
             error: null,
             isLoaded: false,
             records: []
        }
    }
    componentDidMount() {
        getJSON('http://localhost:4000/record').then(
            response => this.setState({
                records: response,
                isLoaded: true
            }),
            error => this.setState({
                isLoaded: true,
                error
            })
        )
    }
    render() {
        const { error, isLoaded, records } = this.state;
            if (error) {
                return <div > Error: { error.responseText } < /div>
            } else if (!isLoaded) {
                return <div > Loading... < /div>       
            } else {
            	return ( 
                	< div >
                		<h1> Records </h1>
                		<table className = 'table table-bordered'>
                			<thead>
                				<tr>
                					<td > Date </td> 
                					<td> Title </td> 
                					<td > Amount </td>  
                				</tr> 
                			</thead> 

                			<tbody> 
                    			{this.state.records.map((record) => < Record key = { record.id}{...record}/>)}      
        					</tbody >
                      	</table >

                 	</div>
               	)
            }
          }
       }

      export default App
```

#### 4 react中使用axios请求api

安装axios

```javascript
npm i axios -D
```

请求成功消息 response.data

请求失败消息 error.message    捕捉错误信息 .catch

```javascript
import React, { Component } from 'react'
import Record from './record'
import axios from 'axios'
class App extends Component {
    constructor() {
        super()
        this.state = {
            error: null,
            isLoaded: false,
            records: []
        }
    }
    componentDidMount() {
        axios.get('http://localhost:4000/record').then(
            response => this.setState({
                records: response.data,
                isLoaded: true
            })
        ).catch(
            error => this.setState({
                isLoaded: true,
                error
            })
        )
    }
    render() {
            const { error, isLoaded, records } = this.state;
            if (error) {
                return <div > Error: { error.message } < /div>
            } else if (!isLoaded) {
                return <div > Loading... < /div>       
            } else {
                return ( < div >
                    <
                    h1 > Records < /h1> <
                    table className = 'table table-bordered' >
                    <
                    thead >
                    <
                    tr >
                    <
                    td > Date < /td> <
                    td > Title < /td> <
                    td > Amount < /td>  < /
                    tr > <
                    /thead> 

                    <
                    tbody > {
                        records.map((record) => < Record key = { record.id } {...record }
                            />)} < /
                            tbody >

                            <
                            /table >

                            <
                            /div>
                        )
                    }
                }
            }

            export default App
```

axios api

```javascript
//新建axios api的js文件 RecordsAPI.js ,利用json-server模拟接口
import axios from 'axios'
const api = process.env.REACT_APP_RECORDS_API_URL || "http://localhost:4000"
export const creat = (body) => axios.post(`${api}/record`, body)
export const update = (id, body) => axios.post(`${api}/record/${id}`, body)
```

在用到该路径的js文件中如何使用

```javascript
import React, { Component } from 'react'
//1.先引入RecordsAPI
import * as RecordsAPI from './RecordsAPI'
export default class RecordForm extends Component {
    constructor(props) { 
        super(props)
        this.state = {
            date: '',
            title: '',
            amount: '',
            
        }
        
    }
    handleChange(e) { 
        let name, obj;
        name = e.target.name
        this.setState((
            obj = {},
            obj["" + name] = e.target.value,
            obj
        ))
    }
    vaild() { 
        
        return this.state.date && this.state.title && this.state.amount
    }
    handleSubmit(e) {
        e.preventDefault();
        const data = {
            date: this.state.date,
            title: this.state.title,
            amount:Number.parseInt(this.state.amount)
        }
        //2. 调用RecordsAPI.creat
        RecordsAPI.creat(data).then(
            response => { 
                this.props.handleNewRecord(response.data)
                this.setState({
                    date: '',
                    title: '',
                    amount:''
                })
            }
        ).catch(
            error=>console.log(error.message)
        )
     }
    render() {
        return (
            <form className='form-inline mb-3' onSubmit={this.handleSubmit.bind(this)}>
                <div className='form-group mr-1'>
                    <input type='text' className='form-control' onChange={this.handleChange.bind(this)} placeholder='Date' name='date' value={this.state.date}/>
                </div>
                <div className='form-group mr-1'>
                <input type='text' className='form-control' onChange={this.handleChange.bind(this)} placeholder='Title' name='title' value={this.state.title}/>
                </div>
                <div className='form-group mr-1'>
                    <input type='text' className='form-control' onChange={this.handleChange.bind(this)} placeholder='Amount' name='amount' value={this.state.amount}/>
                </div>
                <button type='submit' className='btn btn-primary' disabled={!this.vaild()}>Creat Record</button>
            </form>        

        )
    }
}
```



**注意**

react生成的表单如果要重新渲染其中的value应该改变state才会重新渲染，所以要给input绑定onChange事件 

## react全家桶---Redux

安装

```javascript
npm i redux -S
```

Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

入口文件index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import * as serviceWorker from './serviceWorker';
//1. 在入口文件中引入redux中的createStore变量 
import { createStore } from 'redux'
//2. 引入reducer
import reducer from './reducers/counter'

//3.创建store
const store = createStore(reducer)
// store.subscribe(()=>console.log('State update',store.getState()))
// store.dispatch({
//   type:'INCREASE'
// })

const render = () => { 
  ReactDOM.render(
    <App value={store.getState()} handleIncrease={
      () => store.dispatch({ type: 'INCREASE' })
    } handleDecrease={
      () => store.dispatch({ type: 'DESCREASE' })
    }/>,
    document.getElementById('root')
  );
}

render()

store.subscribe(render)

serviceWorker.unregister();
```








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


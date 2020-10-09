<template>
	<div id="app">
		<div>
			<span>TodoList</span>
			<input type="text" v-model="inputValue" @keyup.enter="submit">
			<button @click="submit">提交</button>
		</div>
		
		<div>
			正在进行中{{doingList}}
			<ul>
				<li v-for="(item,index) in todoList" :key='item.id' v-show="!item.isDone">
					<input type="checkbox" @click.prevent="change(item,true)">
					<span>{{item.content}}</span>
					<button @click="deletetd">删除</button>
				</li>
			</ul>
		</div>
		
		<div>
			已经完成{{doneList}}
			<ul>
				<li v-for="(item,index) in todoList" :key='item.id' v-show="item.isDone">
					<input type="checkbox" @click.prevent="change(item,false)">
					<span>{{item.content}}</span>
					<button @click="deletetd">删除</button>
				</li>
			</ul>
		</div>
		
		
		
	</div>
</template>

<script>
	// import TodoItem from './components/TodoItem.vue'
	export default {
		// name: 'App',
		// components: {
		// 	TodoItem
		// },
		data() {
			return {
				inputValue: '',
				todoList: [],
			}
		},
		created(){
			let todoList = todoList ? JSON.parse(localStorage.todoList) : [];
		},
		computed: {
			doingList() {
				let count = 0;
				this.todoList.map(item => {
					if (!item.isDone) {
						count++;
					}
				})
				return count;
			},
			doneList() {
				let count = 0;
				this.todoList.map(item => {
					if (item.isDone) {
						count++;
					}
				})
				return count;
			}
		},
		methods: {
			submit(){
				if(this.inputValue){
					this.todoList.push({
						content:this.inputValue,
						isDone:false
					})
					this.save();
					this.inputValue = '';					
				}
			},
			save(){
				localStorage.inputValue = JSON.stringify(this.todoList);
			},
			change:function(item,checked){
				if(checked){
					item.isDone = true;
				}else{
					item.isDone = false;
				}
				this.save();
			},
			deletetd(index){
				this.todoList.splice(index,1);
				this.save();
			}
		}
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
</style>

<template>
  	<div class="card_list">
		<table class='table'>
			<tbody>
				<tr>
					<td  width="12%">卡片名称:</td>
					<td width="12%"><input type="text" class="form-control" v-model="name_cont" /></td>
					<td width="12%">卡片号:</td>
					<td width="12%"><input type="text" class="form-control" v-model="card_code_cont" /></td>
					<td width="12%">卡片内容:</td>
					<td width="12%"><input type="text" class="form-control" v-model="content_cont"></td>
					<td width="12%">特征:</td>
					<td width="12%"><input type="text" class="form-control" v-model="feature_cont"></td>
				</tr>
				<tr>
					<td colspan="8">
						<div class="pull-right">
							<input type="button" value="搜索" class="btn btn-primary" @click="search">
							<input type="button" value="清空" class="btn btn-default" @click="clear_search">
						</div>
					</td>
				</tr>
			</tbody>
		</table>	  
		<paginate
				:page-count="total_pages"
				:page-range="3"
				ref="paginate"
				:margin-pages="2"
				:click-handler="clickCallback"
				:prev-text="'上一页'"
				:next-text="'下一页'"
				:container-class="'pagination'"
				:page-class="'page-item'">
		</paginate>
		<span v-show="status=='loading'">正在加载数据...</span>
		<table width="80%" class="table table-condensed table-hover table-striped" >
			<thead style="text-align: center; vertical-align: center">
				<tr>
					<td>卡片</td>
					<td>卡名</td>
					<td>效果</td>
			</tr>
			</thead>
			<tbody>
				<template v-for="card in cards">
					<tr>
						<td><img v-bind:src="host + card.picture.http_url" /></td>
						<td>{{card.name}}</td>
						<td>{{card.content}}</td>
					</tr>	  
				</template>
			</tbody>
		</table>
		
  </div>
</template>

<script>
	import Vue from 'vue'
	import Paginate from 'vuejs-paginate'
	Vue.component('paginate', Paginate)

    export default {
        name: 'CardList',
        data () {
            return {
                status: 'loading',
                cards: [],
				page: 1,
				total_pages: 20,
				host: "http://xiayuanyin.cn:4002",
				//以下搜索相关
				name_cont: "",
				card_code_cont: "",
				content_cont: "",
				feature_cont: ""
            }
        },

        methods: {
			clickCallback(pageNum) {
				this.page = pageNum
				this.get_cards()
			},

			search() {
				this.page = 1
				console.log(this.$refs)
				this.$refs.paginate.selected = 0
				this.get_cards()
			},

			clear_search() {
				this.name_cont = "";
				this.card_code_cont = "";
				this.content_cont = "";
				this.feature_cont = ""
			},

            get_cards() {
                this.status = 'loading'
                let url = "http://xiayuanyin.cn:4002/api/v1/cards?"
				//let url = "http://localhost:3000/api/v1/cards"
				url += `&page=${this.page}`
				var cols = ['name_cont', 'card_code_cont', 'content_cont', 'feature_cont']
				cols.forEach((col) => {
					url += `&${col}=${encodeURI(this[col])}`
				})
				console.log(url)
                fetch(url, {
                    method: "GET",
                    headers: {
                            'Accept': 'application/json'
                    }
                })
                .then((responseData) => {return responseData.json()})
                .then((data) => {
					console.log(data)
					this.cards = data.cards
					this.total_pages = data.meta.total_pages
					this.status = 'loaded'
                })
            }
        },

        created () {
            this.get_cards()
		},

		
    }
</script>

<style>
	
</style>


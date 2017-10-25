<template>
  	<div class="card_list">
		<table width="80%">
			<thead>
				<tr>
					<!-- <th>卡片</th> -->
					<th>卡名</th>
			</tr>
			</thead>
			<tbody>
				<template v-for="card in cards">
					<tr>
						<!-- <td><img v-bind:src="host + card.picture.http_url" /></td> -->
						<td>{{card.name}}</td>
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
				host: "http://xiayuanyin.cn:4002",
				paginate: ['cards']
            }
        },

        methods: {
            get_cards() {
                this.status = 'loading'
                let url = "http://xiayuanyin.cn:4002/api/v1/cards?"
				//let url = "http://localhost:3000/api/v1/cards"
				url += `&page=${this.page}`
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
					this.status = 'loaded'
                })
            }
        },

        created () {
            this.get_cards()
        }
    }
</script>

<style>
	
</style>


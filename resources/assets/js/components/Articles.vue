<template>
	<div>
		<h2>Articles</h2>
		<nav aria-label="Page navigation example">
		  <ul class="pagination">
		    <li v-bind:class="[{disabled : !pagination.pre_page_url}]" class="page-item"><a class="page-link" href="#" @click="fetchArticles(pagination.pre_page_url)" >Previous</a></li>
		    <li v-bind:class="[{disabled : !pagination.next_page_url}]" class="page-item"><a class="page-link" href="#" @click="fetchArticles(pagination.next_page_url)" >Next</a></li>
		  </ul>
		</nav>
		<div class="card card-body mb-2" v-for="article in articles">
			 <h3>{{ article.title }}</h3>
			 <p>{{ article.body }}</p>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				articles: [],
				article: {
					id: '',
					title: '',
					body: ''
				},
				article_id: '',
				pagination: {},
				edit: false
			}
		},

		created(){
			this.fetchArticles();
		},

		methods:{
			fetchArticles(page_url){
				let vm = this;
				page_url = page_url || 'api/articles'
				fetch(page_url)
					.then(res => res.json())
					.then(res => {
						console.log(res.data);
						this.articles = res.data;
						vm.makePagination(res.meta, res.links);
					})
					.catch(err => console.log(err));
			},

			makePagination(meta, links){
				let pagination = {
					current_page: meta.current_page,
					last_page: meta.last_page,
					next_page_url: links.next,
					pre_page_url:links.prev
				}

				this.pagination = pagination;
			}
		}
	}
</script>
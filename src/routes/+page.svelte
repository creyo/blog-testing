<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>
<script>
// @ts-nocheck

	import { onMount } from 'svelte';
	import supabase  from '$lib/config/supabase.js';
 	let articles = [];
	onMount(async () => {
	let { data, error } = await supabase
  			.from('articles')
              .select(`
            *,
            articlestatus(*),
            authors(*),
            categories(*),
            post_type(*),
            publication(*)
          `).eq('publication_id', 0)

		if (error) {
			console.log(error);
		} else {    
			articles = data;
		}
	});
</script>

{#each articles as article}	
	<ul>
		<li><a href={article.url}>{article.title}</a></li>
	</ul>	
			
{/each}
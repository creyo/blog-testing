<svelte:head>
	<title>Blog</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>
<script>
// @ts-nocheck
import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import supabase  from '$lib/config/supabase.js';
 	let article = [];
    let slug = '';
    let body = '';
	onMount(async () => {
        slug = $page.params.slug;   	
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
			article = data.filter((item) => item.url === slug);
            body = article[0].body;
		}
	});
</script>
<div class="container">
    <p>{@html body}</p>
</div>


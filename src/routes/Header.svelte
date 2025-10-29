<script lang='ts'>
    let {
        name
        } : {
            name: string;
        } = $props();
</script>

<h1>Hello {name}</h1>
<h2>{name.replace('o', 'x')}</h2>


<style>
    h1 {
        color: red;
    }
</style>


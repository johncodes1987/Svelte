<script lang='ts'>
    import Header from "./Header.svelte";
    let name = $state('John');

    let status: 'OPEN' | 'CLOSED' = $state('OPEN');

    // function toggle() {
    //     status = status === 'OPEN'? 'CLOSED' : 'OPEN';
    // }
    // Same as below, but using a function to toggle the status instead of a ternary operator.

    function onclick() {
        status = status === 'OPEN'? 'CLOSED' : 'OPEN';
    }

</script>

<Header name={name}/>

<input type='text' bind:value={name} />

<p>The store is now {status}!</p>
<button {onclick}>Toggle Status</button>
<svelte:options customElement="injected-app" />

<style>
    .main-container {
        display: flex;
        justify-content: center;
    }

    .main-grid {
        display: grid;
        width: min-content;
    }

    .main-grid > div {
        margin: 0.2em;
        padding: 4em;
        background: #101010;
        white-space: nowrap;
    }

    .main-grid > div > div {
        font-size: 1.5em;
        text-align: center;
        color: palegreen;
    }
</style>

<script>
    let messageCount = 0;
    let clientCount = 0;
    let runTime = 0;

    const eventSource = new EventSource('/events');

    eventSource.onmessage = (msg) => {
        console.log(msg);
        const parsed = JSON.parse(msg.data);
        messageCount = parsed.messageCount;
        clientCount = parsed.clientCount;
        runTime = parsed.runTime;
    }

    $: sections = [
        ['Message count', messageCount], 
        ['Client count', clientCount], 
        ['Run time', runTime]
    ];
</script>

<div class="main-container">
    <div class="main-grid">
        {#each sections as [label, value] (label)}
            <div><div>{`${label}: ${value}`}</div></div>
        {/each}
    </div>
</div>

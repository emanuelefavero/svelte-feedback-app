<script>
    import { feedback } from '../stores'

    // NOTE: Svelte Reactive Declarations:
    // ($feedback GETS the array from the store)
    // count is a reactive property that is updated by the store
    // whenever the feedback array changes
    $: count = $feedback.length
    $: average =
        $feedback.reduce((accumulator, item) => accumulator + item.rating, 0) /
        $feedback.length
</script>

<div class="feedback-stats">
    <h4>{count} Reviews</h4>
    {#if average}
        <h4>Ratings Average: {average.toFixed(1)}</h4>
    {/if}
</div>

<style>
    .feedback-stats {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>

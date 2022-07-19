<script>
    import { feedback } from '../stores'
    // NOTE: Svelte Transitions
    import { fade, scale } from 'svelte/transition'
    import FeedbackItem from './FeedbackItem.svelte'
</script>

<!-- use $feedback to get feedback array from the store -->
{#each $feedback as fb (fb.id)}
    <div in:scale={{ duration: 400 }} out:fade={{ duration: 300 }}>
        <FeedbackItem item={fb} />
    </div>
{/each}

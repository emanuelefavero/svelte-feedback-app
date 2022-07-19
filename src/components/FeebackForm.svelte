<script>
    import { v4 as uuidv4 } from 'uuid'
    import { feedback } from '../stores'
    import Card from './Card.svelte'
    import Button from './Button.svelte'
    import RatingSelect from './RatingSelect.svelte'

    let text = ''
    let rating = 10
    $: btnDisabled = true
    let min = 10
    let message = ''

    function handleInput() {
        if (text.trim().length <= min) {
            message = `Please enter at least ${min} characters`
            btnDisabled = true
        } else {
            message = null
            btnDisabled = false
        }
    }

    function handleSelect(e) {
        rating = Number(e.detail)
    }

    function handleSubmit(e) {
        if (text.trim().length > min) {
            const newFeedback = {
                id: uuidv4(),
                // TIP: make sure rating is passed as a number by adding a plus sign + before the number, +rating
                rating: +rating,
                text,
            }
            // NOTE: Add newFeedback to the feedback array in the store
            feedback.update((feedback) => [newFeedback, ...feedback])

            // Clear text in input field after submitting
            text = ''
        }
    }
</script>

<Card>
    <header>
        <h2>How would you rate your service with us?</h2>
    </header>
    <form on:submit|preventDefault={handleSubmit}>
        <!-- NOTE: Svelte custom event 'rating-select' from 'RatingSelect' -->
        <RatingSelect on:rating-select={handleSelect} />

        <div class="input-group">
            <input
                type="text"
                on:input={handleInput}
                bind:value={text}
                placeholder="Tell us something that keeps you coming back"
            />
            <Button disabled={btnDisabled} type="submit" style="primary"
                >Send</Button
            >
        </div>
        {#if message}
            <div class="message">
                {message}
            </div>
        {/if}
    </form>
</Card>

<style>
    header {
        max-width: 400px;
        margin: auto;
    }

    header h2 {
        font-size: 22px;
        font-weight: 600;
        text-align: center;
    }

    .input-group {
        display: flex;
        flex-direction: row;
        border: 1px solid #ccc;
        padding: 8px 10px;
        border-radius: 8px;
        margin-top: 15px;
    }

    input {
        flex-grow: 2;
        border: none;
        font-size: 16px;
    }

    input:focus {
        outline: none;
    }

    .message {
        padding-top: 10px;
        text-align: center;
        color: rebeccapurple;
    }
</style>

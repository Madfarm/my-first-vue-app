<template>
    <div class="quote-container">
        <div class="search-area">
            <input type="text" v-model="searchQuery" placeholder="Enter a search term">
            <button @click="searchQuotes">Search</button>
        </div>
        <div class="quote" v-if="quote">
            <p>{{ quote.content }}</p>
            <span class="author">- {{ quote.author }}</span>
        </div>
        <button @click="fetchQuote" v-else>New Random Quote</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            searchQuery: '',
            quote: null,
        };
    },
    methods: {
        async fetchQuote() {
            try {
                const response = await fetch('https://api.quotable.io/random');
                const data = await response.json();
                this.quote = data;
            } catch (error) {
                console.error('Error fetching quote:', error);
                this.quote.content = 'Failed to fetch quote. Try again later.';
                this.quote.author = '';
            }
        },
        async searchQuotes() {
            if (this.searchQuery) {
                try {
                    const response = await fetch(`https://api.quotable.io/search/quotes?query=${this.searchQuery}`);
                    const data = await response.json();

                    if (data.results.length > 0) {
                        this.quote = data.results[0];
                    } else {
                        this.quote = {
                            content: 'No quotes found for your search.',
                            author: ''
                        };
                    }
                } catch (error) {
                    // ... (Error handling, similar to fetchQuote)
                }
            }
        },
        mounted() {
            this.fetchQuote();
        }
    }
};
</script>


<style scoped>
.quote-container {
    text-align: center;
    font-family: sans-serif;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.quote {
    margin-bottom: 10px;
    font-size: 1.2em;
}

.author {
    font-style: italic;
}

button {
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.search-area {
  margin-bottom: 20px;
}
</style>
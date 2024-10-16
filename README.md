﻿import React, { useState } from 'react';
import axios from 'axios';

function Dictionary() {
    const [word, setWord] = useState('');
    const [definition, setDefinition] = useState(null);
    const [error, setError] = useState(null);

    const handleSearch = async (e) => {
        e.preventDefault();
        if (!word) return;
        try {
            const response = await axios.get(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`);
            setDefinition(response.data[0].meanings[0].definitions[0].definition);
            setError(null);
        } catch (err) {
            setError('Word not found');
            setDefinition(null);
        }
    };

    return (
        <div>
            <h1>Dictionary App</h1>
            <form onSubmit={handleSearch}>
                <input
                    type="text"
                    placeholder="Enter a word"
                    value={word}
                    onChange={(e) => setWord(e.target.value)}
                />
                <button type="submit">Search</button>
            </form>
            {definition && <p><strong>Definition:</strong> {definition}</p>}
            {error && <p>{error}</p>}
        </div>
    );
}

export default Dictionary;


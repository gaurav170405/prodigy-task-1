import random
from collections import defaultdict

def build_markov_chain(text, k=1):
    """Build a Markov chain of order k from the given text."""
    markov_chain = defaultdict(list)
    for i in range(len(text) - k):
        current_state = text[i:i+k]
        next_state = text[i+k]
        markov_chain[current_state].append(next_state)
    return markov_chain

def generate_text(markov_chain, start_state, length):
    """Generate text using the given Markov chain starting from the start_state."""
    current_state = start_state
    result = [current_state]
    for _ in range(length - len(start_state)):
        next_states = markov_chain[current_state]
        if not next_states:
            break
        next_state = random.choice(next_states)
        result.append(next_state)
        current_state = current_state[1:] + next_state if len(current_state) > 1 else next_state
    return ''.join(result)



text = "india wins the 2024 T-20 world cup"



markov_chain = build_markov_chain(text, k=1)


start_state = text[:1] 
generated_text = generate_text(markov_chain, start_state, 100)
print(generated_text)


def build_word_markov_chain(text, k=1):
    """Build a Markov chain of order k from the given text, at word level."""
    words = text.split()
    markov_chain = defaultdict(list)
    for i in range(len(words) - k):
        current_state = tuple(words[i:i+k])
        next_state = words[i+k]
        markov_chain[current_state].append(next_state)
    return markov_chain

def generate_word_text(markov_chain, start_state, length):
    """Generate text using the given Markov chain starting from the start_state, at word level."""
    current_state = start_state
    result = list(current_state)
    for _ in range(length - len(start_state)):
        next_states = markov_chain[current_state]
        if not next_states:
            break
        next_state = random.choice(next_states)
        result.append(next_state)
        current_state = tuple(result[-len(current_state):])
    return ' '.join(result)


text = "india wins the 2024 T-20 world cup"


word_markov_chain = build_word_markov_chain(text, k=1)


start_state = tuple(text.split()[:1]) 
generated_word_text = generate_word_text(word_markov_chain, start_state, 20)
print(generated_word_text)

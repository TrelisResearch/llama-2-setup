# llama-2-setup
Prompt format, tokenizer format, and padding guide for Llama 2.

Check out the Colab Notebook in this repo for a more interative explanation.

## Llama 2 Tokenizer

The Llama 2 tokenizer has the following special tokens:
```
BOS Token: <s>
EOS Token: </s>
Mask Token: None
Pad Token: None
Unknown Token: <unk>

```

## Adding a pad or mask token
Llama 2 does not have a default Mask or Pad token. You can add one like this:
```
# Check if the pad token is already in the tokenizer vocabulary
if '<pad>' not in tokenizer.get_vocab():
  # Add the pad token
  tokenizer.add_special_tokens({"pad_token":"<pad>"})

#Resize the embeddings
model.resize_token_embeddings(len(tokenizer))

#Configure the pad token in the model
model.config.pad_token_id = tokenizer.pad_token_id

# Check if they are equal
assert model.config.pad_token_id == tokenizer.pad_token_id, "The model's pad token ID does not match the tokenizer's pad token ID!"

# Print the pad token ids
print('Tokenizer pad token ID:', tokenizer.pad_token_id)
print('Model pad token ID:', model.config.pad_token_id)
print('Model config pad token ID:', model.config.pad_token_id)
```

### Llama 2 Prompt Format
The format for Llama 2 prompts is:
```
    user_prompt = 'Howdy!'
    system_prompt = 'You are a helpful assistant.'

    B_SYS, E_SYS = "<<SYS>>\n", "\n<</SYS>>\n\n"
    B_INST, E_INST = "[INST]", "[/INST]"

    # Chat model prompt
    prompt = f"{B_INST} {B_SYS}{system_prompt.strip()}{E_SYS}{user_prompt.strip()} {E_INST}\n\n"
```



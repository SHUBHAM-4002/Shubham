# ✅ Step 1: Install dependencies
!pip install transformers gradio -q

# ✅ Step 2: Import libraries
from transformers import pipeline
import gradio as gr

# ✅ Step 3: Load model pipeline
generator = pipeline("text-generation", model="gpt2")

# ✅ Step 4: Core Functions
def generate_description(product_info):
    prompt = f"Write a professional, engaging product description for:\n{product_info}\n\nDescription:"
    result = generator(prompt, max_length=150, num_return_sequences=1, do_sample=True)
    return result[0]['generated_text'].replace(prompt, "").strip()

def generate_reviews(product_info):
    prompt = f"Generate 3 realistic and unique customer reviews for:\n{product_info}\n\nReviews:"
    result = generator(prompt, max_length=250, num_return_sequences=1, do_sample=True)
    return result[0]['generated_text'].replace(prompt, "").strip()

def full_generator(product_info):
    description = generate_description(product_info)
    reviews = generate_reviews(product_info)
    return description, reviews

# ✅ Step 5: Gradio UI
gr.Interface(
    fn=full_generator,
    inputs="text",
    outputs=["text", "text"],
    title="AI Product Description & Review Generator",
    description="Enter a product name and specs to generate a product description and realistic customer reviews."
).launch()


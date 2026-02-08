import streamlit as st
import google.generativeai as genai

# Using key and the stable 2.5 flash model
genai.configure(api_key=st.secrets["GEMINI_KEY"])
model = genai.GenerativeModel('gemini-2.5-flash')

# ---  THE UI ---
st.title("🚀 StoryStream AI")
st.markdown("#### Turning complex concepts into simple stories.")

topic = st.text_input("Enter a topic (e.g., Photosynthesis, Gravity, AI):", placeholder="Type here...")

if st.button("Generate Story"):
    if topic:
        with st.spinner('Thinking of a story...'):
            # This prompt ensures the "No Theory" rule is followed
            prompt = f"Explain {topic} using only real-world stories and fun analogies. Strictly no theory or textbook definitions."
            try:
                response = model.generate_content(prompt)
                st.success(f"### How to think about {topic}:")
                st.write(response.text)
            except Exception as e:
                st.error("Connection Error. Please check your internet or API key.")
    else:
        st.warning("Please enter a topic first!")

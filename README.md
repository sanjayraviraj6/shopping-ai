# 🛒 AI Shopping Assistant

An intelligent shopping assistant built with Streamlit and LangChain that helps users search for products, analyze images, and place orders using natural language and AI-powered image recognition.

## Features

- **🔍 Natural Language Search**: Search products using conversational language with optional price and organic filters
- **📸 Image-Based Product Discovery**: Upload product images to find similar items in the store
- **⭐ Smart Rating System**: Get product ratings and reviews from the database
- **🛍️ One-Click Ordering**: Place orders directly through the chat interface
- **🤖 AI-Powered Recommendations**: Uses Groq's Llama 3.3 70B model for intelligent product suggestions
- **👁️ Vision Analysis**: Google Gemini 2.5 Flash for advanced product image analysis

## Tech Stack

- **Frontend**: Streamlit 1.28.0+
- **AI Framework**: LangChain 0.1.0+
- **Language Model**: Groq (Llama 3.3 70B)
- **Vision Model**: Google Generative AI (Gemini 2.5 Flash)
- **Database**: SQLite3
- **Environment**: Python 3.8+

## Project Structure

```
shopping-ai/
├── app.py                 # Main Streamlit application
├── shopping_agent.py      # LangChain agent with tools
├── reviews_api.py         # Product rating retrieval
├── setup_db.py           # Database initialization
├── requirements.txt      # Python dependencies
├── store.db              # SQLite database
├── .env                  # Environment variables (not in git)
├── .gitignore            # Git ignore rules
└── resources/            # Product images
    ├── elephant.png
    ├── honey.png
    └── oats.png
```

## Installation

### Prerequisites
- Python 3.8 or higher
- pip or conda

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/sanjayraviraj6/shopping-ai.git
   cd shopping-ai
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   pip install langchain-google-genai  # For Google Gemini vision
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env  # Create from template if available
   ```

   Add your API keys to `.env`:
   ```
   GROQ_API_KEY=your_groq_api_key
   GOOGLE_API_KEY=your_google_api_key
   ```

5. **Initialize the database** (if needed)
   ```bash
   python setup_db.py
   ```

6. **Run the application**
   ```bash
   streamlit run app.py
   ```

   The app will open at `http://localhost:8501`

## Usage

### Text-Based Search
1. Type a natural language query in the chat input, e.g.:
   - "I want organic honey under $15 with 4+ rating"
   - "Show me oat cereals"
   - "Find vegan products under $10"

### Image-Based Search
1. Use the "Shop by Image" sidebar to upload a product photo
2. The AI will analyze the image and find similar products in the store
3. Review the results and order directly from the chat

### Placing an Order
1. After viewing product results, confirm your selection by saying:
   - "Yes" (for single product)
   - "Order #2" (for specific product number)
   - "Get me the first one"

## API Endpoints & Tools

### Agent Tools

The shopping agent has access to these tools:

- **`search_products(query, max_price, is_organic)`**: Search products by keyword with optional filters
- **`get_rating(product_id)`**: Retrieve average rating and review count
- **`checkout(product_id)`**: Place an order and get confirmation
- **`describe_product_image(image_path)`**: Analyze uploaded images and extract product info

## Database Schema

### Products Table
```sql
CREATE TABLE products (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  category TEXT,
  price REAL,
  description TEXT,
  is_organic INTEGER
);
```

### Reviews Table
```sql
CREATE TABLE reviews (
  id INTEGER PRIMARY KEY,
  product_id INTEGER,
  rating REAL,
  FOREIGN KEY(product_id) REFERENCES products(id)
);
```

### Orders Table
```sql
CREATE TABLE orders (
  id INTEGER PRIMARY KEY,
  product_id INTEGER,
  product_name TEXT,
  price REAL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Configuration

Key settings in `shopping_agent.py`:

```python
llm = ChatGroq(model="llama-3.3-70b-versatile", temperature=0)
vision_llm = ChatGoogleGenerativeAI(model="gemini-2.5-flash", temperature=0)
```

Adjust the `temperature` parameter (0.0-1.0) to control response creativity:
- `0.0`: Deterministic, consistent responses
- `1.0`: More creative and varied responses

## Requirements

See `requirements.txt` for all dependencies:
- streamlit
- langchain
- langchain-core
- langchain-groq
- python-dotenv

## Environment Variables

Create a `.env` file in the project root:

```
GROQ_API_KEY=<your_groq_api_key>
GOOGLE_API_KEY=<your_google_api_key>
```

**⚠️ Never commit `.env` to version control!** It's included in `.gitignore` for security.

## Security Notes

- API keys are stored in `.env` and excluded from git
- The SQLite database contains product and order information
- Image uploads are processed locally and in-memory
- Ensure you keep your API keys confidential

## Troubleshooting

### Image Upload Issues
- Ensure the image format is supported (JPG, JPEG, PNG, WebP)
- Check file size isn't too large
- Verify Google API key has vision capabilities enabled

### Product Search Returns No Results
- Try broader search terms
- Check that the database is properly initialized
- Verify products exist in the `store.db`

### API Key Errors
- Confirm both `GROQ_API_KEY` and `GOOGLE_API_KEY` are set in `.env`
- Check API keys haven't expired or been revoked
- Ensure you have sufficient API credits

## Future Enhancements

- [ ] User authentication and order history
- [ ] Product recommendations based on browsing history
- [ ] Payment integration
- [ ] Inventory management dashboard
- [ ] Multi-language support
- [ ] Advanced filtering (brand, reviews range, availability)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the MIT License.

## Contact

For questions or issues, please open an issue on GitHub or contact the project maintainer.

---

**Built with ❤️ using Streamlit, LangChain, and AI**

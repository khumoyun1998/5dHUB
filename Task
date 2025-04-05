from fastapi import FastAPI, HTTPException, Request
from pydantic import BaseModel, HttpUrl
from starlette.responses import RedirectResponse
import hashlib
import asyncio

app = FastAPI()

# In-memory storage for shortened URLs
url_db = {}

class URLRequest(BaseModel):
    url: HttpUrl

@app.post("/", status_code=201)
async def shorten_url(request: URLRequest):
    """Shortens the given URL and returns an identifier."""
    url_hash = hashlib.md5(request.url.encode()).hexdigest()[:6]  # Generate a short hash
    url_db[url_hash] = request.url
    return {"shortened_url": f"http://127.0.0.1:8080/{url_hash}"}

@app.get("/{shorten_url_id}", status_code=307)
async def redirect_to_original(shorten_url_id: str):
    """Redirects to the original URL based on the shortened ID."""
    original_url = url_db.get(shorten_url_id)
    if not original_url:
        raise HTTPException(status_code=404, detail="URL not found")
    return RedirectResponse(url=original_url)

@app.get("/async-data")
async def async_request():
    """Simulates an asynchronous operation before returning data."""
    await asyncio.sleep(2)  # Simulate async operation
    return {"message": "Async operation completed"}

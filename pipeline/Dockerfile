# Start with slim Python 3.13 image
FROM python:3.13.10-slim

# Copy uv binary from official uv image (multi-stage build pattern)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# Set working directory
WORKDIR /app

# Add virtual environment to PATH so we can use installed packages
ENV PATH="/app/.venv/bin:$PATH"    
# by adding this line, we dont need the ENTRYPOINT to be ["uv", "run", "python", "pipeline.py"] later

# Copy dependency files first (better layer caching)
COPY pyproject.toml uv.lock .python-version ./
# Install dependencies from lock file (ensures reproducible builds) (don't install our .py script as a package/dependency)
RUN uv sync --locked --no-install-project 

# Copy application code
COPY pipeline.py pipeline.py

# Set entry point
ENTRYPOINT ["python", "pipeline.py"]





# Original (for learning purposes)
#------------------------------------
# # base Docker image that we will build on
# FROM python:3.13.11-slim

# # set up our image by installing prerequisites; pandas in this case
# RUN pip install pandas pyarrow

# # set up the working directory inside the container
# WORKDIR /code
# # copy the script to the container. 1st name is source file, 2nd is destination
# COPY pipeline.py .

# # define what to do first when the container runs
# # in this example, we will just run the script
# ENTRYPOINT ["python", "pipeline.py"]
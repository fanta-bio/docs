# fanta-bio Documentation Website

This repository contains the source code for the fanta-bio documentation website. This site is built using [Hugo](https://gohugo.io/), a fast and flexible static site generator.

## Prerequisites

- **Git**: To clone and commit to this repository.
- **Hugo**: Version **0.124.0** or higher is required. [Install Hugo](https://gohugo.io/installation/).

## Obtaining the Source Code

1. **Clone the repository**:

   ```bash
   git clone https://github.com/fanta-bio/docs.git fanta-bio-docs
   ```

2. **Navigate to the project directory**:

   ```bash
   cd fanta-bio-docs
   ```

## Local Development

To run the website locally:

1. **Start the Hugo server**:

   ```bash
    server -w -p 8080
   ```

   The `-w` flag enables live reloading, and the `-p` flag specifies the port number. You can change the port number to any other available port.

2. **View the site**:

   Open your web browser and go to `http://localhost:8080/`.

   The site supports live reloading, so any changes you make will automatically refresh in the browser.

## Editing Guide

For detailed instructions on how to edit the documentation pages please refer [Editing Guide](http://docs.fanta.bio/editing-guide/).

## License

This project is licensed under the [MIT License](LICENSE).

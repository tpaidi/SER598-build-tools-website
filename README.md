Main branch is built and continuously deployed on Netlify here- ![SER 421- Build Tools Tutorial Website](https://ser421buildtools.netlify.app/)

[![Netlify Status](https://api.netlify.com/api/v1/badges/f2c8a557-1ad0-4179-b930-06dc39bee8f0/deploy-status)](https://app.netlify.com/sites/ser421buildtools/deploys)

# How to install and run this development server using Hugo

---

## **1. Install Hugo**
### **Windows / Mac / Linux**
1. Visit the official [Hugo Installation Guide](https://gohugo.io/getting-started/installing/).
2. Download and install the version suitable for your operating system.

### **Verify Installation**
- Run the following command to verify the installation:

  ```
  hugo version
  ```

---

## **2. Clone the Site Repository**
- Use the following commands to clone the repository and navigate into the project directory, since the repository includes a git submodule pass the `--recurse-submodules` flag to clone it too:

  ```
  git clone --recurse-submodules https://github.com/tpaidi/SER598-build-tools-website.git
  cd SER598-build-tools-website
  ```

---

## **3. Run the Development Server**
- Start the development server by running:

  ```
  hugo server
  ```

### **Expected Output:**
- You should see a message like this:

  ```
  Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
  Press Ctrl+C to stop the server.
  ```

---

## **5. Open the Site in a Browser**
- Open [http://localhost:1313](http://localhost:1313) in your browser to view the site live.

---

## **6. Helpful Commands**

- **View All Hugo Commands:**  
  Display a list of available commands and options:

  ```
  hugo --help
  ```


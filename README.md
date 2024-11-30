social-media-app/
│
├── client/  (React Frontend)
├── server/  (Node.js Backend)
└── database/  (MongoDB)
npx create-react-app client
import React, { useState, useEffect } from "react";

function App() {
  const [posts, setPosts] = useState([]);
  const [newPost, setNewPost] = useState("");

  // Fetch posts from the server
  useEffect(() => {
    fetch("http://localhost:5000/posts")
      .then((res) => res.json())
      .then((data) => setPosts(data));
  }, []);

  const handlePost = () => {
    fetch("http://localhost:5000/posts", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ content: newPost }),
    })
      .then((res) => res.json())
      .then((data) => setPosts([...posts, data]));
    setNewPost("");
  };

  return (
    <div>
      <h1>Social Media App</h1>
      <div>
        <input
          value={newPost}
          onChange={(e) => setNewPost(e.target.value)}
          placeholder="Write a post..."
        />
        <button onClick={handlePost}>Post</button>
      </div>
      <ul>
        {posts.map((post, index) => (
          <li key={index}>{post.content}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
npm init -y
npm install express mongoose cors
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

// MongoDB connection
mongoose
  .connect("mongodb://localhost:27017/socialmedia", { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log("MongoDB Connected"))
  .catch((err) => console.log(err));

// Post schema
const PostSchema = new mongoose.Schema({
  content: String,
});

const Post = mongoose.model("Post", PostSchema);

// API Endpoints
app.get("/posts", async (req, res) => {
  const posts = await Post.find();
  res.json(posts);
});

app.post("/posts", async (req, res) => {
  const post = new Post({ content: req.body.content });
  await post.save();
  res.json(post);
});

// Start server
app.listen(5000, () => console.log("Server running on http://localhost:5000"));
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

// MongoDB connection
mongoose
  .connect("mongodb://localhost:27017/socialmedia", { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log("MongoDB Connected"))
  .catch((err) => console.log(err));

// Post schema
const PostSchema = new mongoose.Schema({
  content: String,
});

const Post = mongoose.model("Post", PostSchema);

// API Endpoints
app.get("/posts", async (req, res) => {
  const posts = await Post.find();
  res.json(posts);
});

app.post("/posts", async (req, res) => {
  const post = new Post({ content: req.body.content });
  await post.save();
  res.json(post);
});

// Start server
app.listen(5000, () => console.log("Server running on http://localhost:5000"));
cd server
node index.js
cd client
npm start
 Chat-With-Me

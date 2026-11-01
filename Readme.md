# Graph Visualiser

An interactive web tool to build graphs on a canvas and watch BFS, DFS, Prim's MST and Dijkstra run on them step by step.

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![MUI](https://img.shields.io/badge/MUI-007FFF?style=for-the-badge&logo=mui&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)

## About

Graph Visualiser is a learning aid for graph algorithms. You draw a graph in the browser, choose an algorithm, and the app animates the resulting traversal or path by highlighting nodes and edges in order. The algorithms are written in Java; a small Express server runs them and returns the result to the React front end.

## Features

- **Choose the graph type**: directed or undirected, weighted or unweighted
- **Build the graph interactively** on a vis-network canvas: add vertices (labelled `V 0`, `V 1`, ...), add edges with optional weights, edit edge weights (weighted graphs) and delete edges; duplicate edges and self-loops are rejected
- **Run algorithms**:
  - Breadth-First Search
  - Depth-First Search
  - Prim's Minimum Spanning Tree
  - Dijkstra's shortest path between a chosen source and destination (weighted graphs)
- **Animated results**: nodes and edges on the returned path are highlighted in sequence, then reset
- Toast notifications on errors, plus a refresh button to start over

## How It Works

1. The client converts the canvas into an adjacency matrix (symmetric for undirected graphs), encoded as rows separated by commas and values separated by spaces, e.g. `0 1 0 1,1 0 1 0,...`.
2. It calls the backend on `http://localhost:3001` with that string as the `graph` query parameter.
3. The Express server runs the matching Java program (`java BFS.java <graph>` and so on) with `child_process.exec`.
4. The Java program prints the node order as space-separated integers, which the server returns as an array for the client to animate.

## Tech Stack

| Part | Technology |
| --- | --- |
| Frontend | React 18 (Create React App), vis-network / vis-data, Tailwind CSS, Material UI, axios, react-hot-toast, react-icons |
| Backend | Node.js, Express, cors |
| Algorithms | Java (BFS, DFS, Prim's, Dijkstra) |

## Project Structure

```
Graph-Visualiser/
├── backend/
│   ├── index.js         # Express server (port 3001) that runs the Java programs
│   ├── BFS.java
│   ├── DFS.java
│   ├── Prims.java
│   └── Dijkstra.java
├── client/
│   └── src/
│       ├── App.jsx
│       └── components/
│           ├── Header.jsx
│           └── Graph.jsx    # canvas, controls, matrix conversion, animation
├── BFS.java, DFS.java, Dijkstra.java, GraphOne.java   # standalone algorithm versions
└── Readme.md
```

## Getting Started

### Prerequisites

- Node.js and npm
- A JDK (11 or newer, to run single-file `java File.java` programs) on your `PATH`

### Backend

```bash
npm i -g nodemon
cd backend
npm install
npm run dev        # nodemon index.js, serves http://localhost:3001
```

### Frontend

```bash
cd client
npm install
npm start          # http://localhost:3000
```

## API Endpoints

All endpoints are `GET` on `http://localhost:3001` and return `{ message, stdout: number[], stderr }`.

| Endpoint | Query parameters | Result |
| --- | --- | --- |
| `/bfs` | `graph` | BFS visit order |
| `/dfs` | `graph` | DFS visit order |
| `/prims` | `graph` | Prim's MST order |
| `/dijkstra` | `graph`, `source`, `destination` | Shortest path from source to destination |

## Author

**Aryan Bhandari**: [GitHub](https://github.com/aryanbha123)

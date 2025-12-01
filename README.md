# Real-Time Deformable Mesh Cutting Simulator

A high-performance, interactive soft-body physics simulation built with **React**, **TypeScript**, and **HTML5 Canvas**. This project demonstrates advanced concepts in computational physics, including Verlet integration, mass-spring systems, and real-time dynamic topology modification.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue)
![React](https://img.shields.io/badge/React-18.0-blue)

## 🌟 Key Features

### 1. Physics-Based Cloth Simulation
- **Mass-Spring System:** The mesh is modeled as a grid of particles connected by structural and shear springs, creating a material that resists stretching and shearing.
- **Verlet Integration:** Uses implicit velocity calculation (`x - px`) for unconditional numerical stability, allowing the simulation to remain robust even under extreme deformation.
- **Iterative Constraint Solving:** Implements a Position-Based Dynamics (PBD) approach, solving spring constraints multiple times per frame to simulate stiffness accurately.

### 2. Realistic Interaction Models
- **"Soft Grab" Deformation:** Unlike simple point-dragging, this simulator implements a radial influence model. Grabbing a particle applies a weighted force to neighboring particles based on distance, creating organic folds, wrinkles, and fabric bunching.
- **Dynamic Cutting:** A ray-casting algorithm detects intersections between the user's "scalpel" vector and mesh edges. Springs and triangles are dynamically deactivated (severed) in real-time, allowing the mesh to fall apart or tear naturally.

### 3. Visual Feedback
- **Stress Visualization:** Springs dynamically change color from blue (relaxed) to red (high stress) based on their current extension relative to their rest length.
- **Visual Aids:** Includes custom rendering for the grab radius, cut paths, and mesh topology.

## 🛠 Technical Architecture

The core simulation loop runs inside a `requestAnimationFrame` cycle within a React component.

### The Physics Engine (`DeformableMesh` Class)
- **Particles:** Store position `(x, y)` and previous position `(px, py)`.
- **Springs:** Enforce distance constraints between particle pairs.
- **Triangles:** Used solely for rendering the continuous surface; they are disabled if their supporting springs are cut.

### Algorithms
- **Integration Step:**
  ```typescript
  vx = (x - px) * damping
  x_new = x + vx + force * dt
  ```
- **Constraint Relaxation:**
  Springs are solved iteratively. If a spring is stretched beyond `tearThreshold`, it snaps (sets `active = false`).
- **Collision Detection:**
  Simple AABB boundary checks keep the cloth within the canvas view.

## 🚀 Getting Started

### Prerequisites
- Node.js (v16 or higher)
- npm or yarn

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/mesh-cutting-simulator.git
   cd mesh-cutting-simulator
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Run the development server**
   ```bash
   npm start
   # or
   yarn start
   ```

4. Open [http://localhost:3000](http://localhost:3000) to view the simulation.

## 🎮 Controls

| Interaction | Action |
|------------|--------|
| **Deform Mode** | **Click & Drag** to grab the cloth. The yellow circle indicates the area of influence. |
| **Cut Mode** | **Click & Drag** to draw a red line. Any mesh edge crossed by the line will be severed. |
| **Pause/Play** | Toggle the physics time-step. |
| **Reset** | Restore the mesh to its original grid state. |

## 🔮 Future Improvements
- [ ] GPU acceleration using WebGL/WebGPU for higher particle counts.
- [ ] Self-collision detection to prevent the cloth from passing through itself.
- [ ] 3D rendering using Three.js or React Three Fiber.
- [ ] Adaptive remeshing (adding vertices along cut lines) for smoother cuts.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

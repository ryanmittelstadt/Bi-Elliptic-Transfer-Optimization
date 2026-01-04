# Description

I was intrigued by the counter-intuitive nature of the Bi-elliptic transfer. That is, by actually traveling further away from a target orbit before returning to it, one can sometimes save propellant. I chose to analyze this problem to define the specific conditions under which this complex maneuver becomes more efficient than the standard Hohmann transfer.
<br />

**The Mission:** Transfer a satellite from an initial circular orbit at 500 km altitude to a target circular orbit at 1,000 km altitude using a minimum amount of Delta-V ($\Delta V$).
<br />

**The Curiosity:** While the Hohmann transfer is often the standard for efficiency, theoretical orbital mechanics suggests that for very large orbital radius ratios, adding a third maneuver at a high intermediate altitude ($R_{b}$) can be superior. I wanted to mathematically prove whether this was the case for Low Earth Orbit (LEO) transfers.

## 1. Mathematical Modeling and Velocity Components

The maneuver involves calculating velocities across three distinct elliptical trajectories: departing the initial orbit, reaching an intermediate peak ($R_{b}$), and finally circularizing at the target orbit.
<br />

The fundamental tool used for this analysis is the Vis-viva equation, which links velocity ($v$) to the current radius ($r$) and the semi-major axis of the orbit ($a$):

$$v^{2} = \mu \left( \frac{2}{r} - \frac{1}{a} \right)$$

To solve the system, I utilized the following variables and objectives:
<br />

* **Objective Function:** Minimize Total $\Delta V = |v_{2} - v_{1}| + |v_{4} - v_{3}| + |v_{6} - v_{5}|$
* **Control Variable:** The intermediate radius ($R_{b}$)
* **Constants:** Earth's gravitational parameter ($\mu$) and radius ($R_{e}$)

## 2. Optimization Strategy: fmincon and fsolve

I employed a two-pronged numerical approach in MATLAB to find the optimal intermediate radius.
<br />

### Constraint-Based Optimization (fmincon)
I used `fmincon` to minimize the Delta-V while adhering to physical bounds ($R_{b} > r_{2}$) and a maximum transfer time constraint.
<br />

* **Finding:** The solver converged on an $R_{b}$ value of 7,379 km for a total $\Delta V$ of 0.263 km/s.

### Root-Solving Refinement (fsolve)
Because the $\Delta V$ gradient can be extremely flat, I implemented an iterative `fsolve` sweep to refine the solution. By analyzing the symbolic derivative of the Delta-V equation with respect to $R_{b}$, I was able to confirm the precision of the optimized results.

## 3. Comparative Analysis & Portfolio Findings

The most significant part of this study was comparing the Bi-elliptic transfer to the standard Hohmann transfer.

<p align="center">
  <img src="https://i.imgur.com/Ur5X8xn.png" height="80%" width="80%" alt="Bi-Elliptic Transfer Delta-V vs. Intermediate Radius"/>
  <br />
  <i>Figure 1: Bi-Elliptic Transfer Delta-V vs. Intermediate Radius (R22)</i>
</p>

**Key Results:**
<br />

* **LEO Case (500km to 1000km):** The Bi-elliptic transfer required 0.263 km/s, while the Hohmann transfer was slightly more efficient at 0.262 km/s.
* **Trade-off in Efficiency:** My analysis proved that for this specific altitude change, the Bi-elliptic maneuver is less efficient and takes significantly longer—1 hour 42 minutes compared to the Hohmann's 49 minutes.
* **The "11.94x - 15.58x Rule":** By extending the simulation to a target orbit of 100,000 km, I visually demonstrated that a Bi-elliptic transfer only becomes efficient when the target radius ($r_{2}$) is significantly greater than the initial radius ($r_{1}$).

<p align="center">
  <img src="https://i.imgur.com/O7q1jU9.png" height="80%" width="80%" alt="Efficiency crossover for r1 to 100,000 km transfer"/>
  <br />
  <i>Figure 2: Efficiency crossover for r1 to 100,000 km transfer</i>
</p>

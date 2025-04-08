# Skyline Algorithm

This project was made as a partial evaluation for the Algotithm Analysis course for the graduation in Computer Science at Cesar School. The goal is to analyze algorithms which use recurrency and see if the Maste Theory is applied or not. 

That said, this presentation explores the Skyline Problem, a classic computational geometry problem, focusing on how to solve the problem using algorithmic techniques like Divide and Conquer, while analyzing its complexity and real-world applications.

## Group
<table align="center">
	<tr>
		<td align="center">
			<a href="https://github.com/anabxalves">
				<img src="https://avatars.githubusercontent.com/u/108446826?v=4" width="200px;" alt="Foto Ana"/><br>
				<sub>
					<b>Ana Beatriz Alves</b>
				</sub>
			</a>
		</td>
		<td align="center">
			<a href="https://github.com/Caiobadv">
				<img src="https://avatars.githubusercontent.com/u/117755420?v=4" width="200px;" alt="Foto Caio"/><br>
				<sub>
					<b>Caio Barreto</b>
				</sub>
			</a>
		</td>
		<td align="center">
			<a href="https://github.com/bela975">
				<img src="https://avatars.githubusercontent.com/u/113048987?v=4" width="200px;" alt="Foto Isabela"/><br>
				<sub>
					<b>Isabela Spinelli</b>
				</sub>
			</a>
		</td>
		<td align="center">
			<a href="https://github.com/MaluArr">
				<img src="https://avatars.githubusercontent.com/u/99887403?v=4" width="200px;" alt="Foto Maria Luisa"/><br>
				<sub>
					<b>Maria Luísa</b>
				</sub>
			</a>
		</td>
		<td align="center">
			<a href="https://github.com/VictorHTenorio">
				<img src="https://avatars.githubusercontent.com/u/101901740?v=4" width="200px;" alt="Foto Victor"/><br>
				<sub>
					<b>Victor Hora</b>
				</sub>
			</a>
		</td>
	</tr>
</table>

## Directory Structure

	├── presentation.pdf	# PPT used to present the project
	├── skyline.ipynb		# Notebook used to exemplify the solution
	├── requirements.txt
	├── .gitignore
	├── LICENSE
	└── README.md

## The Problem
Imagine you are walking through a city and looking at the horizon, seeing only the outline of the buildings. That line that draws the silhouette of the buildings against the sky — that is the city skyline.
But what if the only information someone gave you about the buildings were coordinates, in a format where each building is represented by a tuples (left, height, right)? The goal is to determine the critical points where the skyline changes height.

## Conceptual Solution
A hands-on activity helps understand the problem without code:
- Draw the X-axis: the reference line for building bases;
- Mark Buildings: plot vertical bars according to each tuple;
- Identify Critical Points: where the height changes (up or down);
- Skyline Construction: draw lines connecting critical points to form the skyline.

### Example Building:

| Building | Start (X1) | Height (H) | End (X2) |
|----------|------------|------------|----------|
| A        | 2          | 10         | 9        |
| B        | 3          | 15         | 7        |
| C        | 5          | 12         | 12       |
| D        | 13         | 10         | 16       |
| E        | 15         | 8          | 20       |
| F        | 8          | 14         | 13       |

## Divide and Conquer vs Event Queue

### Event Queue & Max Heap (Sweep Line)

Presenting an alternative and efficient algorithm that uses a **priority queue (heap)**:

#### Steps:
1. For each building:
	- Add two events: `(start, -height)` and `(end, height)`.
2. Sort all events by x-coordinate:
	- Start events before end events.
	- Higher buildings first in case of tie.
3. Maintain a **max heap** of active heights.
4. If the maximum height changes → a new skyline point.

#### Heap Simulation Example:
- Start with heap: `[0]` (ground).
- Process events and update the heap.
- If the tallest height changes, add a new point.

### Divide and Conquer

In this version of the algorithm, it recursively splits the list of buildings and merges the partial skylines.

#### Steps:
1. If only one building: simple skyline (start → height → end).
2. If multiple buildings:
	- **Divide** into halves.
	- **Recursively solve** each half.
	- **Merge** the two skylines.

This method leverages the **Master Theorem**, making it a textbook case for Divide & Conquer.

## Implementation using Divide and Conquer (Example)

```python
def skyline_rec(pontos):
	if len(pontos) <= 1:
		return pontos

	meio = len(pontos) // 2
	esquerda = pontos[:meio]
	direita = pontos[meio:]

	skyline_esq = skyline_rec(esquerda)
	skyline_dir = skyline_rec(direita)

	resultado = []
	for p in skyline_esq + skyline_dir:
		if not any(domina(outro, p) for outro in resultado):
			resultado = [outro for outro in resultado if not domina(p, outro)]
			resultado.append(p)

	return resultado

def domina(p1, p2):
	return all(x <= y for x, y in zip(p1, p2)) and any(x < y for x, y in zip(p1, p2))
```

## Complexity Analysis

After defining the algorithm structure, we use the Master Theorem to analyze the recurrence:

`T(n) = aT(n/b) + f(n)`

For the Divide and Conquer version:

- `a = 2` , given that it divides into two subproblems;
- `b = 2` , each half is of size `n/2`,
- `f(n) = Θ(n)` , linear merging cost.

### Applying the theorem:

Since `f(n) = Θ(n^log_b(a)) = Θ(n^1) = Θ(n)`, this fits Case 2 of the Master Theorem.

### Final Time Complexity:
`T(n) = Θ(n log n)`

## Usages

The algorithmic solution for the Skyline Problem not only resolves it, but also opens up possibilities for solving other problems, such as:

- **Use in Dataset Analysis:**
	- In multidimensional datasets, we sometimes want to detect the data points that have the most influence in a particular dimension. However, they are not always dominated by other points, making them "independent."
	- Skyline points
		- Enable the extraction of insights that are directly aligned with the purpose of the analysis.
		- Example: determining the best restaurant based on the highest points in both "rating" and "price".
		- Also useful for detecting anomalies in time series datasets.
- **Use in Anomaly Detection in Machine Learning**  
- **Monitoring performance**
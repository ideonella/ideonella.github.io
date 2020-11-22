---
layout: post
title: "Algorithm Visualization 1"
date: 2020-11-22
---

<script>
			const canvas = document.getElementById("canvas");
			const ctx = canvas.getContext("2d");
			
			let board = [];
			let openSet = [];
			let closedSet = [];
			let start, end, path = [], over = false;
			let current, interval, paused = true;
			let width = 100, height = 50;
			
			canvas.width = 10 * width;
			canvas.height = 10 * height;
			
			class Spot
			{
				constructor(x, y)
				{
					this.f = this.g = this.h = 0;
					this.x = x, this.y = y;
					this.neighbours = [];
					this.previous = null;
					this.isObstacle = Math.random() < 0.3;
				}
			}
			
			for (let x = 0; x < width; x++)
			{
				board.push([]);
				for (let y = 0; y < height; y++)
				{
					board[board.length - 1][y] = new Spot(x, y);
				}
			}
			
			function heuristic(a, b)
			{
				return dist(a.x, a.y, b.x, b.y);
			}
			
			function dist(x1, y1, x2, y2)
			{
				let a = x1 - x2;
				let b = y1 - y2;
				
				return Math.abs(a) + Math.abs(b);
			}
			
			start = board[0][0];
			end = board[width - 1][height - 1];
			
			start.isObstactle = false;
			end.isObstacle = false;
			
			openSet.push(start);

			function displayBoard(board)
			{
				ctx.clearRect(0, 0, canvas.width, canvas.height);
				
				ctx.fillStyle = "grey";
				ctx.fillRect(0, 0, canvas.width, canvas.height);
				
				for (let arr in board)
				{
					for (let tile in board[arr])
					{
						let t = board[arr][tile];
						
						ctx.fillStyle = t.isObstacle ? "white" : "black";
						ctx.fillRect(t.x * 10 + 2, t.y * 10 + 2, 9, 9);
					}
				}
				
				for (let tile of closedSet)
				{
					ctx.fillStyle = "rgba(255, 0, 0, 1)";
					ctx.fillRect(tile.x * 10 + 2, tile.y * 10 + 2, 9, 9);
				}
				
				for (let tile of openSet)
				{
					ctx.fillStyle = "rgba(0, 255, 0, 1)";
					ctx.fillRect(tile.x * 10 + 2, tile.y * 10 + 2, 9, 9);
				}
				
				let temp = current;
				path = [temp];

				while (temp.previous)
				{
					path.push(temp.previous);
					temp = temp.previous;
				}
				
				for (let tile of path)
				{
					ctx.fillStyle = "rgba(0, 0, 255, 1)";
					ctx.fillRect(tile.x * 10 + 2, tile.y * 10 + 2, 9, 9);
				}
			}
			
			function update()
			{
				if (openSet.length === 0)
				{
					console.log("No solution");
					
					document.getElementById("status").innerHTML = "Status: No solution";
					
					clearInterval(interval);
				}
				else
				{
					let winner = 0;
					
					for (let i = 0; i < openSet.length; i++)
					{
						if (openSet[i].f < openSet[winner].f)
						{
							winner = i;
						}
					}
					
					current = openSet[winner];
					
					if (current === end)
					{
						let temp = current;
						path = [temp];
						
						while (temp.previous)
						{
							path.push(temp.previous);
							temp = temp.previous;
						}
						
						console.log("Solution found");
						
						document.getElementById("status").innerHTML = "Status: Solution found";
						clearInterval(interval);
					}
					
					closedSet.push(current);
					removeFromArray(openSet, current);
					
					for (let n of current.neighbours)
					{
						if (closedSet.includes(n)) { continue }
						
						let tempG = current.g + 1;
						
						if (openSet.includes(n))
						{
							if (tempG < n.g)
							{
								n.g = tempG;
							}
						}
						else
						{
							n.g = tempG;
							openSet.push(n);
						}
						
						n.h = heuristic(n, end);
						n.f = n.g + n.h;
						n.previous = current;
					}
				}
				
				displayBoard(board);
			}
			
			function removeFromArray(array, element)
			{
				for (let i = array.length - 1; i > -1; i--)
				{
					if (array[i] === element)
					{
						array.splice(i, 1);
					}
				}
			}
			
			function addNeighbours()
			{
				for (let x = 0; x < width; x++)
				{
					for (let y = 0; y < height; y++)
					{
						let spot = board[x][y];
						
						let left  = getTileAtPos(x - 1, y);
						let right = getTileAtPos(x + 1, y);
						let up    = getTileAtPos(x, y - 1);
						let down  = getTileAtPos(x, y + 1);
						
						left && !left.isObstacle && spot.neighbours.push(left);
						right && !right.isObstacle && spot.neighbours.push(right);
						up && !up.isObstacle && spot.neighbours.push(up);
						down && !down.isObstacle && spot.neighbours.push(down);
					}
				}
			}
			
			addNeighbours();
			
			function getTileAtPos(x, y)
			{
				if (typeof board[x] === "undefined" ||
					typeof board[x][y] === "undefined")
				{
					return false;
				}
				
				return board[x][y];
			}
			
			window.addEventListener("keydown", event =>
			{
				if (event.key === " ")
				{
					pause();
				}
			});
			
			function pause()
			{
				if (over) { return }
				paused = !paused;
				document.getElementById("status").innerHTML = paused ? "Status: Paused" : "Status: Ongoing";
			}
			
			interval = window.setInterval(function()
			{
				!paused && update();
			}, 25);
			
			displayBoard(board);
		</script>
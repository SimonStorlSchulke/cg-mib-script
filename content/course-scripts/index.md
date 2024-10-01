---
title: Codeschnipsel aus der Vorlesung
---

## Vorlesung 06.05.24

**Terrain Meshgenerierung #1**  
{{<highlight gdscript>}}
@tool # ermöglicht es Skripte im Editor (ohne dass das Spiel gestartet werden muss) auszuführen
extends MeshInstance3D

@export var width = 1.0
@export var height = 1.0
@export var noise_frequency = 0.5
@export var noise_amplitude = 0.1
@export var subdivisions = 10

@export var regenerate_mesh: bool:
 set(value):
  create_mesh()

var noise = FastNoiseLite.new()

func create_mesh():
 noise.seed = randi()
 var arrmesh = ArrayMesh.new()
 var arrays = []
 
 arrays.resize(Mesh.ARRAY_MAX)
 
 var verts = PackedVector3Array()
 
 var cellsize_x = width / subdivisions
 var cellsize_z = height / subdivisions
 
 for x in subdivisions:
  var p_row = x * cellsize_x
  for z in subdivisions:
   var p_column = z * cellsize_z

   # Wir generieren die vier Eckpunkte einer Zelle...
   var p1 = Vector3(p_row, height_map(p_row, p_column), p_column)
   var p2 = Vector3(p_row + cellsize_x,  height_map(p_row + cellsize_x, p_column), p_column)
   var p3 = Vector3(p_row,  height_map(p_row, p_column + cellsize_z), p_column + cellsize_z)
   var p4 = Vector3(p_row + cellsize_x,  height_map(p_row + cellsize_x, p_column + cellsize_z), p_column + cellsize_z)

   #... und übergeben diese dem verts - array um zwei dreiecke zu bilden.
   # zwei der Eckpunkte werden hier daher doppelt verwendet 
   verts.append_array([p1, p2, p3, p2, p4, p3])
 
 arrays[Mesh.ARRAY_VERTEX] = verts
 
 arrmesh.add_surface_from_arrays(Mesh.PRIMITIVE_TRIANGLES, arrays)
 mesh = arrmesh


func height_map(x: float, z: float) -> float:
 noise.fractal_octaves = 5
 noise.frequency = noise_frequency
 return noise.get_noise_2d(x, z) * noise_amplitude
{{</highlight>}}

## Vorlesung 10.06. - Jump & Run
{{<highlight gdscript>}}
extends Node3D

@onready var area3D: Area3D = $Area3D

@export var speed: float = 1.0

@export var jumpHeight: float = 2

@export var gravitation: float = 0.5

var currentFallSpeed: float = 0.0

var maxFallSpeed: float = 10


func walk(directtion: int, delta: float):
 position.z += speed * directtion * delta

func _input(event):
 if(event.is_action_pressed("jump")):
  jump()

func jump():
 currentFallSpeed = jumpHeight


func calculate_fall_speed(delta: float):

 var overlappingAreas = area3D.get_overlapping_areas()

 var nextFallSpeed = currentFallSpeed - gravitation * delta

 if(len(overlappingAreas) > 0 && currentFallSpeed < 0):
  var platform = overlappingAreas[0].get_parent()
  nextFallSpeed = 0
  position.y = platform.position.y

 if(currentFallSpeed < 0):
  nextFallSpeed *= 1.1
 currentFallSpeed = clamp(nextFallSpeed, -maxFallSpeed, maxFallSpeed)


func _physics_process(delta):
 calculate_fall_speed(delta)

 position.y += currentFallSpeed * delta

 if Input.is_action_pressed("walk_left"):
  walk(1, delta)
 if Input.is_action_pressed("walk_right"):
  walk(-1, delta)
{{</highlight>}}

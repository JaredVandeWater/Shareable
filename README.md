# fade-code
extends Node2D

onready var fade = $colorRect
export(int, "Fade In", "Fade Out") var FadeType
export var fadeColor = Color( 1, 1, 1, 1 )
export var fadeSpeed = 0.5
var isCurrentlyFading = true
signal fadeComplete

func _ready():
	fade.color = fadeColor
	position.x = 0
	position.y = 0
	z_index = 100
	fade.rect_size.y = OS.window_size.y
	fade.rect_size.x = OS.window_size.x
	
func _process(delta):
	if isCurrentlyFading:
		_handleFading(delta)
		
func startFade():
	isCurrentlyFading = true
		
func _handleFading(delta):
	if FadeType == 0: 
		if fade.modulate.a > 0:
			fade.modulate.a -= fadeSpeed * delta
		if fade.modulate.a <= 0:
			emit_signal("fadeComplete")
			print("emit")
			isCurrentlyFading = false
	if FadeType == 1:
		if fade.modulate.a < 1:
			fade.modulate.a += fadeSpeed * delta
		if fade.modulate.a >= 1:
			emit_signal("fadeComplete")
			print("emit")
			isCurrentlyFading = false

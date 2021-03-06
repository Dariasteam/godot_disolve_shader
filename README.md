# Zelda BOTW like disolve shader for Godot Engine 3.1

![](shader.gif)

There are two versions, one made with the visual shader editor tool and another using code, for the sphere and the cube respectively.

Code version:
```
shader_type spatial;
render_mode cull_disabled, depth_draw_alpha_prepass;

uniform sampler2D noise_text;
uniform vec3 color;
uniform float velocity_factor;
uniform float border_size;

void fragment () {
	vec4 noise = texture(noise_text, UV);
	float input = sin(TIME * velocity_factor);
	
	float cut_pass    = (noise.r >               input) ? 1.0 : 0.0;
	float border_pass = (noise.r - border_size > input) ? 0.0 : 1.0;
	
	ALPHA = cut_pass;
	EMISSION = vec3(border_pass) * color;
}
```


Old Code Version (more similar to the visual shader one):
```
shader_type spatial;
render_mode cull_disabled, depth_draw_alpha_prepass;

uniform sampler2D noise_text;
uniform vec3 color;
uniform float velocity_factor;

void fragment () {
	vec4 noise = texture(noise_text, UV);
	float input = sin(TIME * velocity_factor) / 2.0;
	
	float cut_pass = noise.r + input;	
	float border_pass = cut_pass - 0.015;
	
	cut_pass = round(cut_pass);
	ALPHA = cut_pass;
		
	border_pass = round (border_pass);
	border_pass = 1.0 - border_pass;
	
	vec3 final_border = vec3(border_pass); 
	final_border *= color;
		
	EMISSION = final_border;
}
```

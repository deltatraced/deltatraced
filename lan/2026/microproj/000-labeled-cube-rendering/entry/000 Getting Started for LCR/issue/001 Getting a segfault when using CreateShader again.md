---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[002 Setup openGL rendering app in rust and do some dev]]'
context_type: issue
status: done
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [002 Setup openGL rendering app in rust and do some dev](../task/002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md)

Spawned in: [^spawn-issue-423b4f](../task/002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md#spawn-issue-423b4f)

# 1 Repro

Have the command

````rust
cmd!("repro", "reproduces an issue", |_, _| {
	println!("Attempt to create new shader:");
	unsafe { 
		let vertexShader = gl::CreateShader(gl::VERTEX_SHADER); 
		println!("Created {vertexShader}");
	}
	Ok("".to_owned())
})
````

Then run it:

````
cargo run --example expt000_hello_circle
| sys repro
````

This should segfault if we have done `gl::CreateShader` already.

# 2 Journal

2026-05-14 Wk 20 Thu - 20:49 +03:00

Would like to be able to update the resolution dynamically from the shell, but we segfault if we do this, even if there is a mutex that ensures that the shell program is accessed one code region at a time.

[wikis.khronos.org on VAOs VBOS Vertex and Fragment Shaders](https://wikis.khronos.org/opengl/Tutorial2:_VAOs,_VBOs,_Vertex_and_Fragment_Shaders_(C_/_SDL)).

[crates.io gl](https://crates.io/crates/gl). [crates.io glfw](https://crates.io/crates/glfw).

[stackexchange on segmentation faults](https://unix.stackexchange.com/questions/277331/segmentation-fault-core-dumped-to-where-what-is-it-and-why).

````sh
ulimited -c unlimited
cargo run --example expt000_hello_circle # reproduce segfault
ulimited -c 0
````

The file is located in `/var/lib/apport/coredump/` for me.

We can inspect it with gdb:

````
gdb {exec} /var/lib/apport/coredump/{core}

# in gdb
info stack
````

````rust
pub fn build_shader_program_for_circle_or_fail(resolution: u32) -> ShaderProgramData {
    let (shader_program, vao, num_indices) = unsafe {
        let created_shader_data =
 /*-->*/    unsafe_create_and_link_shaders(vertexShaderSource, fragmentShaderSource);
````

We can tell the segfault happens here after putting logs at each stage of this function.

And then here between `U1` and `U2`:

````rust
pub fn unsafe_create_and_link_shaders(
    vertex_shader_source: &str,
    fragment_shader_source: &str,
) -> CreatedShaderData {
    unsafe {
        // build and compile our shader program
        // ------------------------------------
        // vertex shader
        println!("U1");
        let vertexShader = gl::CreateShader(gl::VERTEX_SHADER);
        println!("U2")
````

So it can be reproduced with

````rust
cmd!("repro", "reproduces an issue", |_, _| {
	println!("Attempt to create new shader:");
	unsafe { 
		let vertexShader = gl::CreateShader(gl::VERTEX_SHADER); 
		println!("Created {vertexShader}");
	}
	Ok("".to_owned())
})
````

Same thing if you use `gl::FRAGMENT_SHADER`.

[OpenGL Ref gl4 glCreateShader](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glCreateShader.xhtml)

2026-05-15 Wk 20 Fri - 04:29 +03:00

We can actually call `gl::CreateShader` as much as we want in the render loop. I put it there also to test if this might be due to it being called from a different thread (shell thread).

Okay, by sending the new data as a message, the render thread now is able to update the shader program through user control, although currently it's inefficient in response time.

---
title: "mach/gpu: the gpu.Interface for Zig"
description: "mach/gpu: native, low-level graphics for Zig via WebGPU (DirectX 12, Metal, and Vulkan.) Get started in under 60s with these examples."
draft: false
layout: "unlimited"
rss_ignore: true
images: ["https://raw.githubusercontent.com/hexops/media/b71e82ae9ea20c22a2eb3ab95d8ba48684635620/gpu/logo_light.svg"]
---

<style>
.p-warning {
    text-align: center;
    padding: 0;
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
    background: red;
    margin-left: -1rem;
    margin-right: -1rem;
}
@media (prefers-color-scheme: light) {
    .p-warning, .p-warning a {
        color: white;
    }
}
.p-section {
    display: flex;
    flex-direction: row;
    margin-top: 3rem;
    align-items: center;
    justify-content: center;
}
.p-section-highlight {
    margin-top: 4rem;
    margin-bottom: 2rem;
}
.p-section-right {
    margin-left: 3rem;
}
.p-section-left {
    margin-right: 3rem;
}
.p-img-left {
    height: 10rem;
    margin-left: 4.5rem;
}
.p-img-right {
    height: 10rem;
    margin-right: 4.5rem;
}
.p-logo { margin-right: 3rem; margin-top: 2rem; }
.p-logo>img {
    height: 10rem;
    width: 100%;
}

h2 {
    text-align: left;
    margin-top: 0;
}

.code-inline {
    display: inline-block;
    padding-top: 0.25rem;
    padding-bottom: 0.25rem;
}
.code-inline>a {
    color: black;
}

.code {
    text-align: left;
    background: #c2c2c2;
    color: black;
    padding: 0.5rem;
    font-weight: bold;
}

.code::-moz-selection { /* Code for Firefox */
  color: white;
  background: black;
}

.code::selection {
  color: white;
  background: black;
}

@media (max-width:700px) {
    .p-warning { margin-top: 0; }
    .p-logo { margin: auto; margin-top: 0; margin-bottom: -2rem; }
    .p-logo img { margin-top: -1rem; }
    .p-section { margin-top: 4rem; flex-direction: column; }
    .p-section h2 { text-align: center; }
    .p-section-right { margin-left: 0; }
    .p-section-left { margin-right: 0; }
    .p-section small { margin: 0; display: block; text-align: center; }
    .p-img-left { margin: auto; margin-top: 2rem; margin-bottom: -1rem; height: 6rem; }
    .p-img-right { margin: auto; margin-bottom: 2rem; margin-top: -1rem; height: 6rem; }
    .p-section iframe {
        width: 100%;
        height: 15rem;
    }
}

.animated-demo > video, .static-demo > img {
    width: 20rem;
}
</style>

<div class="p-warning">
    Mach is still early stages - see <a href="/#early-stages">what we have today</a> and <em><a href="https://twitter.com/machengine">stay tuned.</a></em>
</div>

<picture>
    <source srcset="https://raw.githubusercontent.com/hexops/media/b71e82ae9ea20c22a2eb3ab95d8ba48684635620/gpu/logo_dark.svg" media="(prefers-color-scheme: dark)">
    <img style="max-height: 9rem; width:80%; margin: 1rem;" src="https://raw.githubusercontent.com/hexops/media/b71e82ae9ea20c22a2eb3ab95d8ba48684635620/gpu/logo_light.svg">
</picture>

<h1 style="margin-top: 0; margin-bottom: 0;" align="center">The WebGPU interface for Zig</h1>
<div class="p-section" style="align-items: baseline; margin-top: 0;">
    <div class="p-section-right">
        <h2 style="margin-top: 3rem;">Mach core vs. engine</h2>
        <p>All examples below are <em><strong>Mach core</strong></em> examples: Mach provides truly-cross-platform window+input+WebGPU API, nothing else! These examples show how to leverage WebGPU in Zig for graphics.</p>
        <p>These are not <em><strong>Mach engine</strong></em> examples (which would be higher-level): they're low-level, Mach just handles the cross-platform bits (providing you with browser, mobile, and more support in the future.)</p>
    </div>
    <div>
        <h2 style="margin-top: 3rem;">mach/gpu</h2>
        <p>mach/gpu leverages Google Chrome's WebGPU implementation behind the scenes: <a href="https://devlog.hexops.com/2022/mach-v0.1-zig-graphics-in-60s#behind-the-scenes">we do some heavy lifting</a> to give you effortless cross-compilation and make it all 'just work' with no hassle in under 60 seconds on Windows/Linux/macOS.</p>
        <p>WebGPU is an up-and-coming low-level graphics API like Vulkan, Metal, and DirectX 12. In the future, Mach will provide higher-level _Mach engine_ examples with model rendering, text rendering, etc. as optional libraries on built on top. You get to pick & choose what you use!</p>
    </div>
</div>

<small style="margin-top: 3rem;">
    All examples require Zig nightly v0.11+, see <a href="https://github.com/hexops/mach/blob/main/doc/known-issues.md">known issues</a>.
</small>

<div class="p-section">
    <a class="animated-demo" href="/core-example/advanced-gen-texture-light.mp4">
        <video autoplay loop>
        <source src="/core-example/advanced-gen-texture-light.mp4" type="video/mp4">
        </video>
    </a>
    <div class="p-section-right">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/advanced-gen-texture-light">advanced-gen-texture-light example</a></h2>
        <p>Generates a brick texture at comptime, uses Blinn-Phong lighting, and several pipelines. Move camera with arrow keys / WASD.</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-advanced-gen-texture-light
</pre></code>
        </div>
        <small>~770 lines of code</small>
    </div>
</div>

<div class="p-section">
    <div class="p-section-left">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/textured-cube">textured-cube example</a></h2>
        <p>Loads a PNG image and uploads the texture to the GPU. Renders it on a 3D cube.</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-textured-cube
</pre></code>
        </div>
        <small>~310 lines of code</small>
    </div>
    <a class="animated-demo" href="/core-example/textured-cube.mp4">
        <video autoplay loop>
        <source src="/core-example/textured-cube.mp4" type="video/mp4">
        </video>
    </a>
</div>

<div class="p-section">
    <a class="animated-demo" href="/core-example/cubemap.mp4">
        <video autoplay loop>
        <source src="/core-example/cubemap.mp4" type="video/mp4">
        </video>
    </a>
    <div class="p-section-right">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/cubemap">cubemap example</a></h2>
        <p>Renders a cubemap / skybox. Nothing fancy, but these are instrumental as backgrounds in games.</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-cubemap
</pre></code>
        </div>
        <small>~370 lines of code</small>
    </div>
</div>

<div class="p-section">
    <div class="p-section-left">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/boids">boids example</a></h2>
        <p>Uses a GPU compute shader to run calculations / simulate flocking behaviour of birds.</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-boids
</pre></code>
        </div>
        <small>~280 lines of code</small>
    </div>
    <a class="animated-demo" href="/core-example/boids.mp4">
        <video autoplay loop>
        <source src="/core-example/boids.mp4" type="video/mp4">
        </video>
    </a>
</div>

<div class="p-section">
    <a class="static-demo" href="/core-example/image-blur.png">
        <img src="/core-example/image-blur.png" />
    </a>
    <div class="p-section-right">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/image-blur">image-blur example</a></h2>
        <p>Leverages a compute shader to blur an image, then renders it. Don't worry if the details are a bit fuzzy!</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-image-blur
</pre></code>
        </div>
        <small>~320 lines of code</small>
    </div>
</div>

<div class="p-section">
    <div class="p-section-left">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/triangle">triangle example</a></h2>
        <p>This is where you should start learning. It tells the GPU to render 3 vertices (but doesn't transfer them using a vertex buffer or anything! The vertex positions are hard-coded in the shader.)</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-triangle
</pre></code>
        </div>
        <small>~70 lines of code</small>
    </div>
    <a class="static-demo" href="/core-example/triangle.png">
        <img src="/core-example/triangle.png" />
    </a>
</div>

<div class="p-section">
    <a class="animated-demo" href="/core-example/rotating-cube.mp4">
        <video autoplay loop>
        <source src="/core-example/rotating-cube.mp4" type="video/mp4">
        </video>
    </a>
    <div class="p-section-right">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/rotating-cube">rotating-cube example</a></h2>
        <p>Uploads a basic 3D cube to the GPU and renders it. Demonstrates how to use vertex buffers to transfer a model from the CPU to GPU, how to use vertex attributes & more.</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-rotating-cube
</pre></code>
        </div>
        <small>~220 lines of code</small>
    </div>
</div>

<div class="p-section">
    <div class="p-section-left">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/two-cubes">two-cubes example</a></h2>
        <p>Once you've learned how to render one cube, two is just 30 lines of code more!</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-two-cubes
</pre></code>
        </div>
        <small>~250 lines of code</small>
    </div>
    <a class="animated-demo" href="/core-example/two-cubes.mp4">
        <video autoplay loop>
        <source src="/core-example/two-cubes.mp4" type="video/mp4">
        </video>
    </a>
</div>

<div class="p-section">
    <a class="animated-demo" href="/core-example/instanced-cube.mp4">
        <video autoplay loop>
        <source src="/core-example/instanced-cube.mp4" type="video/mp4">
        </video>
    </a>
    <div class="p-section-right">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/instanced-cube">instanced-cube example</a></h2>
        <p>EVEN MORE CUBES! Instancing lets you duplicate an object & render it in multiple places with different parameters.</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-instanced-cube
</pre></code>
        </div>
        <small>~230 lines of code</small>
    </div>
</div>

<div class="p-section">
    <div class="p-section-left">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/fractal-cube">fractal-cube example</a></h2>
        <p>Cube-inception! Renders the scene to a texture, which is then rendered on the rotating cube itself as a texture!</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-fractal-cube
</pre></code>
        </div>
        <small>~390 lines of code</small>
    </div>
    <a class="animated-demo" href="/core-example/fractal-cube.mp4">
        <video autoplay loop>
        <source src="/core-example/fractal-cube.mp4" type="video/mp4">
        </video>
    </a>
</div>

<div class="p-section">
    <a class="static-demo" href="/core-example/triangle-msaa.png">
        <img src="/core-example/triangle-msaa.png" />
    </a>
    <div class="p-section-right">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/triangle-msaa">triangle-msaa example</a></h2>
        <p>Remember that triangle from before? If we turn on MSAA (Multi-Sample Anti Aliasing) the edges will become <em>smooooth.</em></p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-triangle-msaa
</pre></code>
        </div>
        <small>~110 lines of code</small>
    </div>
</div>

<div class="p-section">
    <div class="p-section-left">
        <h2><a href="https://github.com/hexops/mach-examples/tree/main/core/map-async">map-async example</a></h2>
        <p>Some of the best examples have <em>no graphics</em>. This one shows how to transfer data to the GPU, perform computations on that data using the GPU's parallel processing capbilities, and get results back on the CPU. If you're interested in GPU compute, this is the place to start!</p>
        <div>
<code><pre class="code">
git clone --recursive https://github.com/hexops/mach-examples
cd mach-examples/
zig build run-map-async
</pre></code>
        </div>
        <small>~80 lines of code</small>
    </div>
    <a class="static-demo" href="/core-example/map-async.png">
        <img src="/core-example/map-async.png" />
    </a>
</div>


<div class="p-section">
    <img class="p-img-left" src="/img/wrench.svg"></img>
    <div class="p-section-right">
        <h2>Join the community!</h2>
        <p>Join the <a href="https://discord.gg/XNG3NZgCqp">Discord server</a> - we'd love to have you there and are always happy to help!</p>
        <p>We're also looking for more contributors to <a href="https://github.com/hexops/mach/issues/230">help us build and port WebGPU examples</a> for Zig. If you prefer reading about WebGPU, there's plenty of great learning material online about <a href="https://surma.dev/things/webgpu/">compute</a> and rendering[<a href="https://twitter.com/Tojiro/status/1549852280270102528">0</a>][<a href="https://alain.xyz/blog/raw-webgpu">1</a>].</p>
        <p>Special thanks to these people who've contributed to these examples:</p>
        <div style="max-width: 40rem; text-align: left; margin-top: 1rem;">
            <a href="https://github.com/alichraghi"><img src="https://images.weserv.nl/?url=github.com/alichraghi.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/Andoryuuta"><img src="https://images.weserv.nl/?url=github.com/Andoryuuta.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/d3m1gd"><img src="https://images.weserv.nl/?url=github.com/d3m1gd.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/iddev5"><img src="https://images.weserv.nl/?url=github.com/iddev5.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/johanfforsberg"><img src="https://images.weserv.nl/?url=github.com/johanfforsberg.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/lucasromanosantos"><img src="https://images.weserv.nl/?url=github.com/lucasromanosantos.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/NewbLuck"><img src="https://images.weserv.nl/?url=github.com/NewbLuck.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
            <a href="https://github.com/PiergiorgioZagaria"><img src="https://images.weserv.nl/?url=github.com/PiergiorgioZagaria.png?v=4&h=60&w=60&fit=cover&mask=circle&maxage=7d" width="60px" alt="" /></a>
        </div>
    </div>
</div>

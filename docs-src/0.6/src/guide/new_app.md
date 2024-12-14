
## Create a new project

You can create a new Dioxus project by running the following command and following the prompts:

```sh
dx new hot_dog
```

![dx new](/assets/06_docs/dx_new_06.mp4)

You'll need to select a template to use to get started.

- Bare-bones: a very simple setup with just a `main.rs` an and `assets` folder.
- Jumpstart: a scaffolded app with components, views, and suggested structure.
- Workspace: a full cargo workspace setup with different crates per platform.

We're going to use the bare-bones template for *HotDog*. Our app won't be too complex and can fit in one file.

You'll also need to select a "default platform" to run. This will set the corresponding `default = []` feature section in your Cargo.toml and inform `dx` to server that platform by default. For the sake of this tutorial, *HotDog* will be a web app by default, though we will create desktop and mobile builds as well.

We're not going to set up the project with a Router at first, though we will eventually add the router to the app.

We're not going to use TailwindCSS for this tutorial, but make sure to read the [TailwindCSS guide](../cookbook/tailwind.md) if you want to use Tailwind for your own apps.

## Running the project

Once you have created your project, you can start it with the following command:

```sh
cd hot_dog
dx serve
```

![dx serve](/assets/06_docs/dx_serve_06.mp4)

This will start the cargo build and launch a web server to serve your app. If you visit the "serve" address (in this case, `http://127.0.0.1:8080`), then you'll receive a nice loading screen in your browser:

![dx serve](/assets/06_docs/hotdog_loading.png)

Once the app is loaded, you should be greeted with the default Dioxus template:


![dx serve](/assets/06_docs/default_dioxus_app.png)

Congrats! You have your very first Dioxus App.

## Structure of the app

Open the app in your favorite editor and take a look at its structure:

```sh
├── Cargo.lock
├── Cargo.toml
├── Dioxus.toml
├── README.md
├── assets
│   ├── favicon.ico
│   ├── header.svg
│   └── main.css
└── src
    └── main.rs
```

All Rust apps are comprised of a root `Cargo.toml` and a `main.rs` file located in the `src` folder. Our CLI `dx` pre-filled these files with the `dioxus` dependency and some starter code for us to get building quickly.

## The Cargo.toml

The `Cargo.toml` outlines the dependencies to our app and specifies compiler settings. All Rust apps are *compiled*: we execute the Rust tool `cargo` which aggregates our `.rs` files together and generates a final binary executable (like a `.exe`) that runs our app.

All Dioxus apps will include `dioxus` as a dependency:

```toml
[dependencies]
dioxus = { version = "0.6.0", features = [] }
```

The prebuilt Dioxus templates initialize different cargo features for your app. `dx` will use these to decide what cargo features to enable when you specify the `--platform` feature when serving. For example, when building your app for desktop, `dx` will call `cargo build --no-default-features --features desktop`.

```toml
[features]
default = ["web"]
web = ["dioxus/web"]
desktop = ["dioxus/desktop"]
mobile = ["dioxus/mobile"]
```

Starting with Dioxus 0.6, `dx` will also initialize separate [Cargo profiles](https://doc.rust-lang.org/cargo/reference/profiles.html) for your app. These profiles let you customize the optimization level of each platform. `dx` also uses these platforms as a mechanism of isolating builds from each other, enabling parallel server and client builds and preventing unnecessary full-rebuilds.

## Dioxus.toml

The `Dioxus.toml` file contains Dioxus-specific configuration for things like bundling and deploying. Before Dioxus 0.5, we used the `Dioxus.toml` to specify asset inclusion and hot-reload watch paths, but as of Dioxus 0.6, these fields are deprecated and replaced by standards like `asset!()` and `.gitignore`.

We won't need to configure the `Dioxus.toml` for our DogApp just yet.

## Assets Folder

To include assets in your Dioxus app, you'll want to use the `asset!()` macro that we'll cover later in the [Styling and Assets](assets.md) chapter. You can include assets from anywhere within your app's file tree, but we recommend using the pregenerated `assets` folder.

## main.rs

Finally, the `main.rs`. The `main.rs` file is the entrypoint of our app, containing the `fn main` function. All Rust executables start their life at `main`.

The `main` of our HotDog app looks like this:

```rust
use dioxus::prelude::*;

fn main() {
    dioxus::launch(App);
}
```

The `launch` function calls the platform-specific `launch` function depending on which feature (web/desktop/mobile) is enabled on `dioxus`. `launch` takes in a root component, typically called `App`.

We'll cover components more in-depth in the [next chapter](component.md).

## Resetting to Basics

The bare-bones template provides us some starter code that we could use as a basis for our app. However, we want to start truly from scratch, so we'll wipe away the `Hero` component and empty the `App` component to its basics:

```rust
use dioxus::prelude::*;

fn main() {
    dioxus::launch(App);
}

#[component]
fn App() -> Element {
    rsx! { "HotDog!" }
}
```
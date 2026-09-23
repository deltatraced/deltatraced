---
context_type: entry
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](../task/005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-entry-30bc97](../task/005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-entry-30bc97)

# Journal

2026-07-01 Wk 27 Wed - 19:08 +03:00

[Silverbullet MIT License](https://github.com/silverbulletmd/silverbullet/blob/main/LICENSE.md)

````html
<!-- Adapted from silverbullet: https://github.com/silverbulletmd/silverbullet/blob/main/LICENSE.md -->
<!-- supports events onCancel, onKeydown -->
<dialog id="dialog1" class="sb-modal-box">
	<!-- supports events onClick -->
	<div id="div-sb-header" class="sb-header">
		<label>{label}</label>
		<!-- supports events onKeyDown, onInput, onKeyUp,   -->
		<input
			class="sb-input" {extra: "sb-filter-input"}
			value=""
		/>
	</div>
	<div
		className="sb-help-text"
		innerHTML=?0
	></div>
	<div
		className="sb-result-list"
		tabIndex={-1}
	>
		{webcomponent eachMatchingOption}
			<!-- supports onMouseMove, onClick, -->
			<div
				className = "sb-option sb-selected-option" OR
							"sb-option" OR
							"sb-option sb-decorated-object {cssClass}" OR
			>
			{if Icon}
				<span
					className="sb-icon"
				>
					{webcomponent icon width={16} height={16}}
				</span>
			<span
				className="sb-name"
			>
				{name with or without prefix}
			</span>
			{if option.hint}
				<span
					className="sb-hint" OR
							  "sb-hint sb-hint-inactive" 
				>
					{option.hint}
				</span>
				<div className="sb-description">{option.description}</div>
			</div>
</dialog>
````

````scss
/* Adapted from silverbullet: https://github.com/silverbulletmd/silverbullet/blob/main/LICENSE.md */

html {
  --ui-accent-color: #464cfc;
  --ui-accent-text-color: var(--ui-accent-color);
  --ui-accent-contrast-color: #eee;

  --modal-color: inherit;
  --modal-background-color: #fff;
  --modal-border-color: rgb(108, 108, 108);
  --modal-backdrop-color: rgba(0, 0, 0, 0.15);
  --modal-header-label-color: var(--ui-accent-text-color);
  --modal-help-background-color: #eee;
  --modal-help-color: #555;
  --modal-selected-option-background-color: var(--ui-accent-color);
  --modal-selected-option-color: var(--ui-accent-contrast-color);
  --modal-hint-background-color: #212476;
  --modal-hint-color: #eee;
  --modal-hint-inactive-background-color: #e1e1e1;
  --modal-hint-inactive-color: #111;
  --modal-description-color: #6b6b6b;
  --modal-selected-option-description-color: #e6e6e6;
}

.sb-modal-box {
  color: var(--modal-color);
  background-color: var(--modal-background-color);
  border: var(--modal-border-color) 1px solid;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 20px 20px;

  .sb-header {
    border-bottom: 1px var(--modal-border-color) solid;

    label {
      color: var(--modal-header-label-color);
    }

    .sb-input {
      font-family: var(--ui-font);
    }
  }

  .sb-help-text {
    background-color: var(--modal-help-background-color);
    border-bottom: 1px var(--modal-border-color) solid;
    color: var(--modal-help-color);
  }

  .sb-result-list {
    .sb-hint:not(.sb-hint-inactive) {
      color: var(--modal-hint-color);
      background-color: var(--modal-hint-background-color);
    }

    .sb-hint.sb-hint-inactive {
      color: var(--modal-hint-inactive-color);
      background-color: var(--modal-hint-inactive-background-color);
    }

    .sb-description {
      color: var(--modal-description-color);
    }

    .sb-selected-option {
      background-color: var(--modal-selected-option-background-color);
      color: var(--modal-selected-option-color);

      .sb-description {
        color: var(--modal-selected-option-description-color);
      }
    }
  }
}
````

Corresponding css, built in ping-pong-score-ts project build pipeline which also uses scss:

````css
html{--ui-accent-color: #464cfc;--ui-accent-text-color: var(--ui-accent-color);--ui-accent-contrast-color: #eee;--modal-color: inherit;--modal-background-color: #fff;--modal-border-color: rgb(108, 108, 108);--modal-backdrop-color: rgba(0, 0, 0, 0.15);--modal-header-label-color: var(--ui-accent-text-color);--modal-help-background-color: #eee;--modal-help-color: #555;--modal-selected-option-background-color: var(--ui-accent-color);--modal-selected-option-color: var(--ui-accent-contrast-color);--modal-hint-background-color: #212476;--modal-hint-color: #eee;--modal-hint-inactive-background-color: #e1e1e1;--modal-hint-inactive-color: #111;--modal-description-color: #6b6b6b;--modal-selected-option-description-color: #e6e6e6}.sb-modal-box{color:var(--modal-color);background-color:var(--modal-background-color);border:var(--modal-border-color) 1px solid;box-shadow:rgba(0,0,0,.35) 0px 20px 20px}.sb-modal-box .sb-header{border-bottom:1px var(--modal-border-color) solid}.sb-modal-box .sb-header label{color:var(--modal-header-label-color)}.sb-modal-box .sb-header .sb-input{font-family:var(--ui-font)}.sb-modal-box .sb-help-text{background-color:var(--modal-help-background-color);border-bottom:1px var(--modal-border-color) solid;color:var(--modal-help-color)}.sb-modal-box .sb-result-list .sb-hint:not(.sb-hint-inactive){color:var(--modal-hint-color);background-color:var(--modal-hint-background-color)}.sb-modal-box .sb-result-list .sb-hint.sb-hint-inactive{color:var(--modal-hint-inactive-color);background-color:var(--modal-hint-inactive-background-color)}.sb-modal-box .sb-result-list .sb-description{color:var(--modal-description-color)}.sb-modal-box .sb-result-list .sb-selected-option{background-color:var(--modal-selected-option-background-color);color:var(--modal-selected-option-color)}.sb-modal-box .sb-result-list .sb-selected-option .sb-description{color:var(--modal-selected-option-description-color)}
````

2026-07-02 Wk 27 Thu - 17:05 +03:00

Currently doesn't work:

````rust
impl UIBundle {
    pub fn new(service: &str, items: &[String]) -> UIBundle {
        let html = format!(r#"

<div>
    <style>
    html{{--ui-accent-color: #464cfc;--ui-accent-text-color: var(--ui-accent-color);--ui-accent-contrast-color: #eee;--modal-color: inherit;--modal-background-color: #fff;--modal-border-color: rgb(108, 108, 108);--modal-backdrop-color: rgba(0, 0, 0, 0.15);--modal-header-label-color: var(--ui-accent-text-color);--modal-help-background-color: #eee;--modal-help-color: #555;--modal-selected-option-background-color: var(--ui-accent-color);--modal-selected-option-color: var(--ui-accent-contrast-color);--modal-hint-background-color: #212476;--modal-hint-color: #eee;--modal-hint-inactive-background-color: #e1e1e1;--modal-hint-inactive-color: #111;--modal-description-color: #6b6b6b;--modal-selected-option-description-color: #e6e6e6}}.sb-modal-box{{color:var(--modal-color);background-color:var(--modal-background-color);border:var(--modal-border-color) 1px solid;box-shadow:rgba(0,0,0,.35) 0px 20px 20px}}.sb-modal-box .sb-header{{border-bottom:1px var(--modal-border-color) solid}}.sb-modal-box .sb-header label{{color:var(--modal-header-label-color)}}.sb-modal-box .sb-header .sb-input{{font-family:var(--ui-font)}}.sb-modal-box .sb-help-text{{background-color:var(--modal-help-background-color);border-bottom:1px var(--modal-border-color) solid;color:var(--modal-help-color)}}.sb-modal-box .sb-result-list .sb-hint:not(.sb-hint-inactive){{color:var(--modal-hint-color);background-color:var(--modal-hint-background-color)}}.sb-modal-box .sb-result-list .sb-hint.sb-hint-inactive{{color:var(--modal-hint-inactive-color);background-color:var(--modal-hint-inactive-background-color)}}.sb-modal-box .sb-result-list .sb-description{{color:var(--modal-description-color)}}.sb-modal-box .sb-result-list .sb-selected-option{{background-color:var(--modal-selected-option-background-color);color:var(--modal-selected-option-color)}}.sb-modal-box .sb-result-list .sb-selected-option .sb-description{{color:var(--modal-selected-option-description-color)}}
    </style>

    <!-- Adapted from silverbullet: https://github.com/silverbulletmd/silverbullet/blob/main/LICENSE.md -->
    <!-- supports events onCancel, onKeydown -->
    <dialog
        className="sb-modal-box"
    >
        <!-- supports events onClick -->
        <div
            className="sb-header"
        >
            <label>Some Label</label>
            <!-- supports events onKeyDown, onInput, onKeyUp,   -->
            <input
                class="sb-input sb-filter-input"
                value="Some input value"
            />
        </div>
        <div
            className="sb-help-text"
        ></div>
        <div
            className="sb-result-list"
            tabIndex={{-1}}
        >
            <div
                className = "sb-option sb-selected-option"
            >
                <span
                    className="sb-name"
                >
                    Some Name
                </span>
            </div>

            <div
                className = "sb-option"
            >
                <span
                    className="sb-name"
                >
                    Some Name
                </span>
            </div>
    </dialog>

    <div>
        <p>Hi!</p>
        <button id="btn1">
            Click me: <span id="count">0</span>
        </button>
        <button id="btn_reset">
            Reset
        </button>
    </div>
</div>


"#);

        let script = format!(r#"
let count = 0;
document.getElementById('btn1').onclick = () => {{
    count++;
    document.getElementById('count').textContent = count;
    const resp_promise = syscall('system.invokeFunction', 'clusterline.post_message', ['{MODULE_TOPIC}', 'btn1_onclick', '{{' +
        '"service": {service}' +
    '}}']);

    resp_promise.then((resp) => {{
        console.log("btn1 awaited response: " + resp);
    }})
}};

document.getElementById('btn_reset').onclick = () => {{
    const resp_promise = syscall('system.invokeFunction', 'clusterline.post_message', ['{MODULE_TOPIC}', 'btn_reset_onclick', '{{' +
        '"service": {service}' +
    '}}']);

    resp_promise.then((resp) => {{
        console.log("btn_reset awaited response: " + resp);
    }})
}};
"#);

        UIBundle { html: html.to_owned() , script: script.to_owned() }
    }
}
````

2026-07-03 Wk 27 Fri - 13:40 +03:00

* `sb-filter-input` is not currently used

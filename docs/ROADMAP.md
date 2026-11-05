# Tay roadmap

Each milestone ends with something you can run. Work through the Rust Book alongside the early
milestones, and start Learn Wgpu around milestone 3.

## M0: Workspace and protocol
- [ ] Cargo workspace with `protocol`, `tayd` and `tay-cli` crates
- [ ] `protocol`: `Request`, `Response` and `Event` enums with serde and `schemars`
- [ ] First three methods: `session.list`, `file.open`, `review.comment`
- [ ] Done when `cargo test` passes and the JSON Schema for each method prints

## M1: Daemon and CLI over a socket
- [ ] `tayd` listens on `$XDG_RUNTIME_DIR/tay/default` with tokio and length-delimited JSON
- [ ] `tay api list`, `tay api describe <method>`, `tay api call <method> '{...}'`
- [ ] Start on demand: `tay` launches `tayd` if no socket answers
- [ ] Exit after about 10 seconds with no clients, unless started with `tayd --foreground`
- [ ] Done when `tay api call session.list` returns JSON from a daemon that `tay` started

## M2: Neovim in the daemon
- [ ] `tayd` spawns `nvim --embed` per open file using `nvim-rs`
- [ ] Attach to the linegrid UI and keep the grid state in the daemon
- [ ] A throwaway CLI client prints grid updates as text
- [ ] Done when you can detach, reattach and see the same buffer state

## M3: First window
- [ ] winit window and wgpu surface; clear to Catppuccin Crust
- [ ] Draw rounded rectangles with a signed distance field shader (the panes)
- [ ] Two panes with a gap; keyboard focus moves between them (`Ctrl-w h/l`)
- [ ] Done when resizing the window keeps the layout correct

## M4: Text
- [ ] cosmic-text shaping and layout, glyph atlas (glyphon or your own)
- [ ] Render a source file in JetBrains Mono with smooth scrolling
- [ ] Syntax highlighting with syntect or tree-sitter
- [ ] Done when a 5,000-line file scrolls without dropped frames

## M5: Neovim pane
- [ ] Client renders the Neovim grid from the daemon (cursor, highlights, mode)
- [ ] Forward keys to Neovim; your config loads
- [ ] Done when you can edit and save a file with your normal Neovim setup inside Tay

## M6: Agent chat
- [ ] `tayd` runs Claude Code through the Agent Client Protocol
- [ ] Stream agent messages to the client; parse markdown as it arrives
- [ ] Render prose, lists, inline code and highlighted code blocks; tool calls as cards
- [ ] Done when a streamed reply renders as styled markdown

## M7: Diff and review
- [ ] Diff tab from git and from the agent's edits
- [ ] Line comments: send now, or batch into a review sent from the composer
- [ ] Edit locks: the agent pauses on a file you open in Neovim; "Hand back to agent"
- [ ] Done when a full review round trip works

## M8: Sidebar, tabs and splits
- [ ] Environments and chats in a sidebar, with status dots
- [ ] Panes hold tabs; split, move tabs between panes; layout saved per chat in the daemon
- [ ] Dia-style connected tabs; glass blur pass
- [ ] Done when switching chats restores each layout, including after restarting the GUI

## M9: Diagrams
- [ ] `tayd` renders Mermaid blocks with `mmdc` to SVG, `resvg` to an image, cached by hash
- [ ] Fallback to the highlighted Mermaid source while rendering or on error

## Later
- Remote environments over Tailscale
- A git worktree per chat
- Command palette
- Pure-Rust Mermaid flowchart layout

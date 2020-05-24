---
title: "Setup foot switch keyboard pedal"
series: "tools"
categories: "notes"
summary: "Give a hand to control the machine"
date: 2020-02-22T21:00:00+01:00
draft: false
tags:
- gadgets
- setup
- keyboard
---

It's a pedal that's used as keyboard to control PC/Mac.

Pedal model is used in this post [Olympus Foot Switch RS28H](https://www.olympus.co.uk/site/en/a/audio_accessories/accessories_professional_dictation/hand_foot_controls/rs28h_footswitch/index.html)

# Olympus Foot Switch RS28H

Why choose this model:

 - Easy to buy from Sweden
 - Support MacOS officially 
 - Easy to setup switch with custom key or shortcuts

# How can setup it

## Install Foot Switch Configuration Tool

The Foot Switch Configuration Tool is management tool that allows you manage your pedal or configuration it.

In time I write this note, it's v100 Windows and v101 MacOS, you can download it from following links:
 - [Download](https://dl-support.olympus-imaging.com/odms_download/ftsw_configuration_tool/en/)
 - [Backup Windows](/files/foot-switch-keyboard-pedal/FTSW_tool_win_V100.zip)
 - [Backup MacOS](/files/foot-switch-keyboard-pedal/FTSW_tool_mac_V101.zip)

Plug Olympus Foot Switch RS28H to your machine, open FTSW tool, setup 

![FTSW tool](/images/foot-switch-keyboard-pedal/ftsw.png)

1. Pedal model
2. Product serial
3. Setting's tab and test pedal
4. Setting's template
5. Configure pedal's swich with keys and food's behaviour
6. Save and apply it to pedal

## Direct mapping

Map pedal to direct key or shortcut keys.

```
                                   ┌─────────────────────┐
                                   │ Browser/Document    │
┌────────┐                         │                     │
│        │                         │         ▲           │
│ Pedal  │────Page Up/Down────────>│         │           │
│        │                         │         │           │
└────────┘                         │         ▼           │
                                   │                     │
                                   └─────────────────────┘
                                   ┌─────────────────────┐
                                   │     Document        │
┌────────┐                         │                     │
│        │                         │  ████████████████   │
│ Pedal  │──────Ctrl + A──────────>│  ███Select All███   │
│        │                         │  ████████████████   │
└────────┘                         │                     │
                                   │                     │
                                   └─────────────────────┘
```


![FTSW settup](/images/foot-switch-keyboard-pedal/ftsw_direct_mapping.png)

## Map to editor's action

Map pedal to a shortcut. The editor maps the shortcut to editor' action.

```
                                    ┌───────────────────────────────┐
                                    │ VIM                           │
                                    │                               │
                                    │                               │
                                    │                               │
┌────────┐                          ├────────────────────┐          │
│        │                          │     Converter      │          │
│ Pedal  │────Ctrl + Shift + B─────>│ Shortcut -> Action │          │
│        │                          │                    │          │
└────────┘                          ├────────────────────┘          │
                                    │          │                    │
                                    │          └─────┐              │
                                    │                V              │
                                    │     ┌────────────────────┐    │
                                    │     │     Save file      │    │
                                    │     └────────────────────┘    │
                                    │                               │
                                    │                               │
                                    │                               │
                                    └───────────────────────────────┘
```

![FTSW settup action](/images/foot-switch-keyboard-pedal/ftsw_snippet.png)

### VIM

```YAML
" ~/.vimrc

map <C-S-b> :w<CR><Esc>
```

### Sublime

```YAML
[
	{"keys": ["ctrl+shift+b"], "command": "insert_snippet", "args": {"contents": "\nif err != nil {\n\n}"}}
]
```

![FTSW settup snippet](/images/foot-switch-keyboard-pedal/sublime_command.png)

## Map to code snippet

Map pedal to shortcut. The editor maps shortcut to plugin.

```
                                    ┌───────────────────────────────┐
                                    │ VIM                           │
                                    │                               │
                                    │                               │
                                    │                               │
┌────────┐                          ├────────────────────┐          │
│        │                          │     Converter      │          │
│ Pedal  │────Ctrl + Shift + B─────>│Shortcut -> Snippet │          │
│        │                          │                    │          │
└────────┘                          ├────────────────────┘          │
                                    │          │                    │
                                    │          └─────┐              │
                                    │                V              │
                                    │     ┌────────────────────┐    │
                                    │     │fmt.Println('Hello')│    │
                                    │     └────────────────────┘    │
                                    │                               │
                                    │                               │
                                    │                               │
                                    └───────────────────────────────┘
```

![FTSW settup snippet](/images/foot-switch-keyboard-pedal/ftsw_snippet.png)

### VIM

```YAML
" ~/.vimrc

command! GolangErrorSnippet call Ignore()
function! Ignore()
   normal! o
   normal! iif err != nil {
   normal! o	abc
   normal! o}
endfunction

map <C-S-b> :GolangErrorSnippet<CR><Esc>
```


### Sublime

```YAML
[
	{"keys": ["ctrl+shift+b"], "command": "insert_snippet", "args": {"contents": "\nif err != nil {\n\n}"}}
]
```

![FTSW settup snippet](/images/foot-switch-keyboard-pedal/sublime_snippet.png)

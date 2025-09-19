# kanata

### config with comments
```
;; ----------------------------------------------------------------------------
;; Kanata Configuration for Vim-like Navigation and Home Row Mods
;; ----------------------------------------------------------------------------

;; -- KEY SOURCES -------------------------------------------------------------
;; Defines the physical layout of your keyboard. Every 'deflayer' block
;; below must have the exact same number of items (60 in this case)
;; as this block.
(defsrc
  grv  1    2    3    4    5    6    7    8    9    0    -    =    bspc
  tab  q    w    e    r    t    y    u    i    o    p    [    ]    \
  caps a    s    d    f    g    h    j    k    l    ;    '    ret
  lsft z    x    c    v    b    n    m    ,    .    /    rsft
  lctl lmet lalt           spc            ralt rmet rctl
)

;; -- VARIABLES ---------------------------------------------------------------
;; Defines reusable variables. Here, we create lists of keys for the left and
;; right hands. These are used by the home row modifiers to prevent them from
;; activating incorrectly when you are typing quickly across the keyboard.
(defvar
  ;; Keys on the right hand. Used by left-hand mods (d, f).
  R-keys (y u i o p [ ] \ h j k l ; ' n m , . /)
  ;; Keys on the left hand. Used by right-hand mods (j, k).
  L-keys (q w e r t a s d f g z x c v b)
)

;; -- ALIASES -----------------------------------------------------------------
;; Aliases are shortcuts for complex actions to keep the layer definitions
;; below clean and readable.
(defalias
  ;; Dual-function Caps Lock:
  ;; - HOLD: Activates the 'navigation' layer for arrow keys.
  ;; - TAP:  The '_' action passes the original 'caps' keypress to the OS.
  ;;         This allows your OS-level remap (Caps -> Esc) to work correctly.
  caps-nav (tap-hold-press 200 200 _ (layer-while-held navigation))

  ;; Home Row Modifiers:
  ;; These keys act as letters when tapped, but as modifiers when held.
  ;; We use 'tap-hold-release-keys' with the hand-specific variables above
  ;; to ensure that fast typing (rolling hands) triggers the letter, not the mod.

  ;; F-key: Tap for 'f', Hold for Left Control.
  f-mod (tap-hold-release-keys 200 200 f lctl $R-keys)
  ;; D-key: Tap for 'd', Hold for Left Alt.
  d-mod (tap-hold-release-keys 200 200 d lalt $R-keys)
  ;; J-key: Tap for 'j', Hold for Right Control.
  j-mod (tap-hold-release-keys 200 200 j rctl $L-keys)
  ;; K-key: Tap for 'k', Hold for Right Alt.
  k-mod (tap-hold-release-keys 200 200 k ralt $L-keys)
)

;; -- LAYERS ------------------------------------------------------------------

;; LAYER 1: QWERTY (Base Layer)
;; This is your main, default typing layer.
(deflayer qwerty
  grv  1    2    3    4    5    6    7    8    9    0    -    =    bspc
  tab  q    w    e    r    t    y    u    i    o    p    [    ]    \
  ;; Caps Lock is mapped to our dual-function nav-layer alias.
  ;; The home row keys d,f,j,k are mapped to their modifier aliases.
  @caps-nav a  s @d-mod @f-mod g    h @j-mod @k-mod l    ;    '    ret
  lsft z    x    c    v    b    n    m    ,    .    /    rsft
  lctl lmet lalt           spc            ralt rmet rctl
)


;; LAYER 2: NAVIGATION
;; This layer is only active while the Caps Lock key is held down.
(deflayer navigation
  ;; Top row: '-' becomes Home, '=' becomes End.
  _    _    _    _    _    _    _    _    _    _    _    home end  _
  _    _    _    _    _    _    _    _    _    _    _    _    _    _
  ;; Home row: h, j, k, l are mapped to Vim-style arrow keys.
  _    _    _    _    _    _    left down up   rght _    _    _
  _    _    _    _    _    _    _    _    _    _    _    _
  _    _    _              _              _    _    _
  ;; All '_' keys are "transparent", meaning they pass through to the qwerty
  ;; layer. This lets all other keys work normally while you hold caps.
)
```

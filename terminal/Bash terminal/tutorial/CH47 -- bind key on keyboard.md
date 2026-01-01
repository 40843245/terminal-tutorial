# CH47 -- bind key on keyboard
## objectives
You will learn how to

    + bind keys on keyboard
    + look at event that binds key on keyboard

## CH47-1 -- look at event that binds key on keyboard
To look at event that binds key on keyboard,

type 

```
bind -P
```

it will list all functionalities and its binding of key on keyboard (each item will list per line).

Also, you can type

```
bind -p
```

it will list all functionalities and its binding of key on keyboard (ordered by functionalites see the following example 2).

### Examples
### Example 1
```
$ bind -P

abort can be found on "\C-g", "\C-x\C-g", "\e\C-g".
accept-line can be found on "\C-j", "\C-m".
alias-expand-line is not bound to any keys
arrow-key-prefix is not bound to any keys
backward-byte is not bound to any keys
backward-char can be found on "\C-b", "\eOD", "\e[D".
backward-delete-char can be found on "\C-h", "\C-?".
backward-kill-line can be found on "\C-x\C-?".
backward-kill-word can be found on "\e\C-h", "\e\C-?".
backward-word can be found on "\e\e[D", "\e[1;5D", "\eb".
beginning-of-history can be found on "\e<", "\e[5~".
beginning-of-line can be found on "\C-a", "\eOH", "\e[1~", "\e[H".
bracketed-paste-begin can be found on "\e[200~".
call-last-kbd-macro can be found on "\C-xe".
capitalize-word can be found on "\ec".
character-search can be found on "\C-]".
character-search-backward can be found on "\e\C-]".
clear-display can be found on "\e\C-l".
clear-screen can be found on "\C-l".
complete can be found on "\C-i", "\e\e\000", "\e[Z".
complete-command can be found on "\e!".
complete-filename can be found on "\e/".
complete-hostname can be found on "\e@".
complete-into-braces can be found on "\e{".
complete-username can be found on "\e~".
complete-variable can be found on "\e$".
copy-backward-word is not bound to any keys
copy-forward-word is not bound to any keys
copy-region-as-kill is not bound to any keys
dabbrev-expand is not bound to any keys
delete-char can be found on "\C-d", "\e[3~".
delete-char-or-list is not bound to any keys
delete-horizontal-space can be found on "\e\\".
digit-argument can be found on "\e-", "\e0", "\e1", "\e2", "\e3", ...
display-shell-version can be found on "\C-x\C-v".
do-lowercase-version can be found on "\C-xA", "\C-xB", "\C-xC", "\C-xD", "\C-xE", ...
downcase-word can be found on "\el".
dump-functions is not bound to any keys
dump-macros is not bound to any keys
dump-variables is not bound to any keys
dynamic-complete-history can be found on "\e\C-i".
edit-and-execute-command can be found on "\C-x\C-e".
emacs-editing-mode is not bound to any keys
end-kbd-macro can be found on "\C-x)".
end-of-history can be found on "\e>", "\e[6~".
end-of-line can be found on "\C-e", "\eOF", "\e[4~", "\e[F".
exchange-point-and-mark can be found on "\C-x\C-x".
fetch-history is not bound to any keys
forward-backward-delete-char is not bound to any keys
forward-byte is not bound to any keys
forward-char can be found on "\C-f", "\eOC", "\e[C".
forward-search-history can be found on "\C-s".
forward-word can be found on "\e\e[C", "\e[1;5C", "\ef".
glob-complete-word can be found on "\eg".
glob-expand-word can be found on "\C-x*".
glob-list-expansions can be found on "\C-xg".
history-and-alias-expand-line is not bound to any keys
history-expand-line can be found on "\e^".
history-search-backward is not bound to any keys
history-search-forward is not bound to any keys
history-substring-search-backward is not bound to any keys
history-substring-search-forward is not bound to any keys
insert-comment can be found on "\e#".
insert-completions can be found on "\e*".
insert-last-argument can be found on "\e.", "\e_".
kill-line can be found on "\C-k".
kill-region is not bound to any keys
kill-whole-line is not bound to any keys
kill-word can be found on "\e[3;5~", "\ed".
magic-space is not bound to any keys
menu-complete is not bound to any keys
menu-complete-backward is not bound to any keys
next-history can be found on "\C-n", "\eOB", "\e[B".
next-screen-line is not bound to any keys
non-incremental-forward-search-history can be found on "\en".
non-incremental-forward-search-history-again is not bound to any keys
non-incremental-reverse-search-history can be found on "\ep".
non-incremental-reverse-search-history-again is not bound to any keys
old-menu-complete is not bound to any keys
operate-and-get-next can be found on "\C-o".
overwrite-mode can be found on "\e[2~".
paste-from-clipboard can be found on "\e[2;2~".
possible-command-completions can be found on "\C-x!".
possible-completions can be found on "\e=", "\e?".
possible-filename-completions can be found on "\C-x/".
possible-hostname-completions can be found on "\C-x@".
possible-username-completions can be found on "\C-x~".
possible-variable-completions can be found on "\C-x$".
previous-history can be found on "\C-p", "\eOA", "\e[A".
previous-screen-line is not bound to any keys
print-last-kbd-macro is not bound to any keys
quoted-insert can be found on "\C-q", "\C-v".
re-read-init-file can be found on "\C-x\C-r".
redraw-current-line is not bound to any keys
reverse-search-history can be found on "\C-r".
revert-line can be found on "\e\C-r", "\er".
self-insert can be found on " ", "!", "\"", "#", "$", ...
set-mark can be found on "\C-@", "\e ".
shell-backward-kill-word is not bound to any keys
shell-backward-word can be found on "\e\C-b".
shell-expand-line can be found on "\e\C-e".
shell-forward-word can be found on "\e\C-f".
shell-kill-word can be found on "\e\C-d".
shell-transpose-words can be found on "\e\C-t".
skip-csi-sequence is not bound to any keys
spell-correct-word can be found on "\C-xs".
start-kbd-macro can be found on "\C-x(".
tab-insert is not bound to any keys
tilde-expand can be found on "\e&".
transpose-chars can be found on "\C-t".
transpose-words can be found on "\et".
tty-status is not bound to any keys
undo can be found on "\C-x\C-u", "\C-_".
universal-argument is not bound to any keys
unix-filename-rubout is not bound to any keys
unix-line-discard can be found on "\C-u".
unix-word-rubout can be found on "\C-w".
upcase-word can be found on "\eu".
vi-append-eol is not bound to any keys
vi-append-mode is not bound to any keys
vi-arg-digit is not bound to any keys
vi-bWord is not bound to any keys
vi-back-to-indent is not bound to any keys
vi-backward-bigword is not bound to any keys
vi-backward-word is not bound to any keys
vi-bword is not bound to any keys
vi-change-case is not bound to any keys
vi-change-char is not bound to any keys
vi-change-to is not bound to any keys
vi-char-search is not bound to any keys
vi-column is not bound to any keys
vi-complete is not bound to any keys
vi-delete is not bound to any keys
vi-delete-to is not bound to any keys
vi-eWord is not bound to any keys
vi-edit-and-execute-command is not bound to any keys
vi-editing-mode is not bound to any keys
vi-end-bigword is not bound to any keys
vi-end-word is not bound to any keys
vi-eof-maybe is not bound to any keys
vi-eword is not bound to any keys
vi-fWord is not bound to any keys
vi-fetch-history is not bound to any keys
vi-first-print is not bound to any keys
vi-forward-bigword is not bound to any keys
vi-forward-word is not bound to any keys
vi-fword is not bound to any keys
vi-goto-mark is not bound to any keys
vi-insert-beg is not bound to any keys
vi-insertion-mode is not bound to any keys
vi-match is not bound to any keys
vi-movement-mode is not bound to any keys
vi-next-word is not bound to any keys
vi-overstrike is not bound to any keys
vi-overstrike-delete is not bound to any keys
vi-prev-word is not bound to any keys
vi-put is not bound to any keys
vi-redo is not bound to any keys
vi-replace is not bound to any keys
vi-rubout is not bound to any keys
vi-search is not bound to any keys
vi-search-again is not bound to any keys
vi-set-mark is not bound to any keys
vi-subst is not bound to any keys
vi-tilde-expand is not bound to any keys
vi-undo is not bound to any keys
vi-unix-word-rubout is not bound to any keys
vi-yank-arg is not bound to any keys
vi-yank-pop is not bound to any keys
vi-yank-to is not bound to any keys
yank can be found on "\C-y".
yank-last-arg can be found on "\e.", "\e_".
yank-nth-arg can be found on "\e\C-y".
yank-pop can be found on "\ey".

```

#### Example 2
```
$ bind -p

"\C-g": abort
"\C-x\C-g": abort
"\e\C-g": abort
"\C-j": accept-line
"\C-m": accept-line
# alias-expand-line (not bound)
# arrow-key-prefix (not bound)
# backward-byte (not bound)
"\C-b": backward-char
"\eOD": backward-char
"\e[D": backward-char
"\C-h": backward-delete-char
"\C-?": backward-delete-char
"\C-x\C-?": backward-kill-line
"\e\C-h": backward-kill-word
"\e\C-?": backward-kill-word
"\e\e[D": backward-word
"\e[1;5D": backward-word
"\eb": backward-word
"\e<": beginning-of-history
"\e[5~": beginning-of-history
"\C-a": beginning-of-line
"\eOH": beginning-of-line
"\e[1~": beginning-of-line
"\e[H": beginning-of-line
"\e[200~": bracketed-paste-begin
"\C-xe": call-last-kbd-macro
"\ec": capitalize-word
"\C-]": character-search
"\e\C-]": character-search-backward
"\e\C-l": clear-display
"\C-l": clear-screen
"\C-i": complete
"\e\e\000": complete
"\e[Z": complete
"\e!": complete-command
"\e/": complete-filename
"\e@": complete-hostname
"\e{": complete-into-braces
"\e~": complete-username
"\e$": complete-variable
# copy-backward-word (not bound)
# copy-forward-word (not bound)
# copy-region-as-kill (not bound)
# dabbrev-expand (not bound)
"\C-d": delete-char
"\e[3~": delete-char
# delete-char-or-list (not bound)
"\e\\": delete-horizontal-space
"\e-": digit-argument
"\e0": digit-argument
"\e1": digit-argument
"\e2": digit-argument
"\e3": digit-argument
"\e4": digit-argument
"\e5": digit-argument
"\e6": digit-argument
"\e7": digit-argument
"\e8": digit-argument
"\e9": digit-argument
"\C-x\C-v": display-shell-version
"\C-xA": do-lowercase-version
"\C-xB": do-lowercase-version
"\C-xC": do-lowercase-version
"\C-xD": do-lowercase-version
"\C-xE": do-lowercase-version
"\C-xF": do-lowercase-version
"\C-xG": do-lowercase-version
"\C-xH": do-lowercase-version
"\C-xI": do-lowercase-version
"\C-xJ": do-lowercase-version
"\C-xK": do-lowercase-version
"\C-xL": do-lowercase-version
"\C-xM": do-lowercase-version
"\C-xN": do-lowercase-version
"\C-xO": do-lowercase-version
"\C-xP": do-lowercase-version
"\C-xQ": do-lowercase-version
"\C-xR": do-lowercase-version
"\C-xS": do-lowercase-version
"\C-xT": do-lowercase-version
"\C-xU": do-lowercase-version
"\C-xV": do-lowercase-version
"\C-xW": do-lowercase-version
"\C-xX": do-lowercase-version
"\C-xY": do-lowercase-version
"\C-xZ": do-lowercase-version
"\eA": do-lowercase-version
"\eB": do-lowercase-version
"\eC": do-lowercase-version
"\eD": do-lowercase-version
"\eE": do-lowercase-version
"\eF": do-lowercase-version
"\eG": do-lowercase-version
"\eH": do-lowercase-version
"\eI": do-lowercase-version
"\eJ": do-lowercase-version
"\eK": do-lowercase-version
"\eL": do-lowercase-version
"\eM": do-lowercase-version
"\eN": do-lowercase-version
"\eP": do-lowercase-version
"\eQ": do-lowercase-version
"\eR": do-lowercase-version
"\eS": do-lowercase-version
"\eT": do-lowercase-version
"\eU": do-lowercase-version
"\eV": do-lowercase-version
"\eW": do-lowercase-version
"\eX": do-lowercase-version
"\eY": do-lowercase-version
"\eZ": do-lowercase-version
"\el": downcase-word
# dump-functions (not bound)
# dump-macros (not bound)
# dump-variables (not bound)
"\e\C-i": dynamic-complete-history
"\C-x\C-e": edit-and-execute-command
# emacs-editing-mode (not bound)
"\C-x)": end-kbd-macro
"\e>": end-of-history
"\e[6~": end-of-history
"\C-e": end-of-line
"\eOF": end-of-line
"\e[4~": end-of-line
"\e[F": end-of-line
"\C-x\C-x": exchange-point-and-mark
# fetch-history (not bound)
# forward-backward-delete-char (not bound)
# forward-byte (not bound)
"\C-f": forward-char
"\eOC": forward-char
"\e[C": forward-char
"\C-s": forward-search-history
"\e\e[C": forward-word
"\e[1;5C": forward-word
"\ef": forward-word
"\eg": glob-complete-word
"\C-x*": glob-expand-word
"\C-xg": glob-list-expansions
# history-and-alias-expand-line (not bound)
"\e^": history-expand-line
# history-search-backward (not bound)
# history-search-forward (not bound)
# history-substring-search-backward (not bound)
# history-substring-search-forward (not bound)
"\e#": insert-comment
"\e*": insert-completions
"\e.": insert-last-argument
"\e_": insert-last-argument
"\C-k": kill-line
# kill-region (not bound)
# kill-whole-line (not bound)
"\e[3;5~": kill-word
"\ed": kill-word
# magic-space (not bound)
# menu-complete (not bound)
# menu-complete-backward (not bound)
"\C-n": next-history
"\eOB": next-history
"\e[B": next-history
# next-screen-line (not bound)
"\en": non-incremental-forward-search-history
# non-incremental-forward-search-history-again (not bound)
"\ep": non-incremental-reverse-search-history
# non-incremental-reverse-search-history-again (not bound)
# old-menu-complete (not bound)
"\C-o": operate-and-get-next
"\e[2~": overwrite-mode
"\e[2;2~": paste-from-clipboard
"\C-x!": possible-command-completions
"\e=": possible-completions
"\e?": possible-completions
"\C-x/": possible-filename-completions
"\C-x@": possible-hostname-completions
"\C-x~": possible-username-completions
"\C-x$": possible-variable-completions
"\C-p": previous-history
"\eOA": previous-history
"\e[A": previous-history
# previous-screen-line (not bound)
# print-last-kbd-macro (not bound)
"\C-q": quoted-insert
"\C-v": quoted-insert
"\C-x\C-r": re-read-init-file
# redraw-current-line (not bound)
"\C-r": reverse-search-history
"\e\C-r": revert-line
"\er": revert-line
" ": self-insert
"!": self-insert
"\"": self-insert
"#": self-insert
"$": self-insert
"%": self-insert
"&": self-insert
"'": self-insert
"(": self-insert
")": self-insert
"*": self-insert
"+": self-insert
",": self-insert
"-": self-insert
".": self-insert
"/": self-insert
"0": self-insert
"1": self-insert
"2": self-insert
"3": self-insert
"4": self-insert
"5": self-insert
"6": self-insert
"7": self-insert
"8": self-insert
"9": self-insert
":": self-insert
";": self-insert
"<": self-insert
"=": self-insert
">": self-insert
"?": self-insert
"@": self-insert
"A": self-insert
"B": self-insert
"C": self-insert
"D": self-insert
"E": self-insert
"F": self-insert
"G": self-insert
"H": self-insert
"I": self-insert
"J": self-insert
"K": self-insert
"L": self-insert
"M": self-insert
"N": self-insert
"O": self-insert
"P": self-insert
"Q": self-insert
"R": self-insert
"S": self-insert
"T": self-insert
"U": self-insert
"V": self-insert
"W": self-insert
"X": self-insert
"Y": self-insert
"Z": self-insert
"[": self-insert
"\\": self-insert
"]": self-insert
"^": self-insert
"_": self-insert
"`": self-insert
"a": self-insert
"b": self-insert
"c": self-insert
"d": self-insert
"e": self-insert
"f": self-insert
"g": self-insert
"h": self-insert
"i": self-insert
"j": self-insert
"k": self-insert
"l": self-insert
"m": self-insert
"n": self-insert
"o": self-insert
"p": self-insert
"q": self-insert
"r": self-insert
"s": self-insert
"t": self-insert
"u": self-insert
"v": self-insert
"w": self-insert
"x": self-insert
"y": self-insert
"z": self-insert
"{": self-insert
"|": self-insert
"}": self-insert
"~": self-insert
"\200": self-insert
"\201": self-insert
"\202": self-insert
"\203": self-insert
"\204": self-insert
"\205": self-insert
"\206": self-insert
"\207": self-insert
"\210": self-insert
"\211": self-insert
"\212": self-insert
"\213": self-insert
"\214": self-insert
"\215": self-insert
"\216": self-insert
"\217": self-insert
"\220": self-insert
"\221": self-insert
"\222": self-insert
"\223": self-insert
"\224": self-insert
"\225": self-insert
"\226": self-insert
"\227": self-insert
"\230": self-insert
"\231": self-insert
"\232": self-insert
"\233": self-insert
"\234": self-insert
"\235": self-insert
"\236": self-insert
"\237": self-insert
"\240": self-insert
"\241": self-insert
"\242": self-insert
"\243": self-insert
"\244": self-insert
"\245": self-insert
"\246": self-insert
"\247": self-insert
"\250": self-insert
"\251": self-insert
"\252": self-insert
"\253": self-insert
"\254": self-insert
"\255": self-insert
"\256": self-insert
"\257": self-insert
"\260": self-insert
"\261": self-insert
"\262": self-insert
"\263": self-insert
"\264": self-insert
"\265": self-insert
"\266": self-insert
"\267": self-insert
"\270": self-insert
"\271": self-insert
"\272": self-insert
"\273": self-insert
"\274": self-insert
"\275": self-insert
"\276": self-insert
"\277": self-insert
"\300": self-insert
"\301": self-insert
"\302": self-insert
"\303": self-insert
"\304": self-insert
"\305": self-insert
"\306": self-insert
"\307": self-insert
"\310": self-insert
"\311": self-insert
"\312": self-insert
"\313": self-insert
"\314": self-insert
"\315": self-insert
"\316": self-insert
"\317": self-insert
"\320": self-insert
"\321": self-insert
"\322": self-insert
"\323": self-insert
"\324": self-insert
"\325": self-insert
"\326": self-insert
"\327": self-insert
"\330": self-insert
"\331": self-insert
"\332": self-insert
"\333": self-insert
"\334": self-insert
"\335": self-insert
"\336": self-insert
"\337": self-insert
"\340": self-insert
"\341": self-insert
"\342": self-insert
"\343": self-insert
"\344": self-insert
"\345": self-insert
"\346": self-insert
"\347": self-insert
"\350": self-insert
"\351": self-insert
"\352": self-insert
"\353": self-insert
"\354": self-insert
"\355": self-insert
"\356": self-insert
"\357": self-insert
"\360": self-insert
"\361": self-insert
"\362": self-insert
"\363": self-insert
"\364": self-insert
"\365": self-insert
"\366": self-insert
"\367": self-insert
"\370": self-insert
"\371": self-insert
"\372": self-insert
"\373": self-insert
"\374": self-insert
"\375": self-insert
"\376": self-insert
"\377": self-insert
"\C-@": set-mark
"\e ": set-mark
# shell-backward-kill-word (not bound)
"\e\C-b": shell-backward-word
"\e\C-e": shell-expand-line
"\e\C-f": shell-forward-word
"\e\C-d": shell-kill-word
"\e\C-t": shell-transpose-words
# skip-csi-sequence (not bound)
"\C-xs": spell-correct-word
"\C-x(": start-kbd-macro
# tab-insert (not bound)
"\e&": tilde-expand
"\C-t": transpose-chars
"\et": transpose-words
# tty-status (not bound)
"\C-x\C-u": undo
"\C-_": undo
# universal-argument (not bound)
# unix-filename-rubout (not bound)
"\C-u": unix-line-discard
"\C-w": unix-word-rubout
"\eu": upcase-word
# vi-append-eol (not bound)
# vi-append-mode (not bound)
# vi-arg-digit (not bound)
# vi-bWord (not bound)
# vi-back-to-indent (not bound)
# vi-backward-bigword (not bound)
# vi-backward-word (not bound)
# vi-bword (not bound)
# vi-change-case (not bound)
# vi-change-char (not bound)
# vi-change-to (not bound)
# vi-char-search (not bound)
# vi-column (not bound)
# vi-complete (not bound)
# vi-delete (not bound)
# vi-delete-to (not bound)
# vi-eWord (not bound)
# vi-edit-and-execute-command (not bound)
# vi-editing-mode (not bound)
# vi-end-bigword (not bound)
# vi-end-word (not bound)
# vi-eof-maybe (not bound)
# vi-eword (not bound)
# vi-fWord (not bound)
# vi-fetch-history (not bound)
# vi-first-print (not bound)
# vi-forward-bigword (not bound)
# vi-forward-word (not bound)
# vi-fword (not bound)
# vi-goto-mark (not bound)
# vi-insert-beg (not bound)
# vi-insertion-mode (not bound)
# vi-match (not bound)
# vi-movement-mode (not bound)
# vi-next-word (not bound)
# vi-overstrike (not bound)
# vi-overstrike-delete (not bound)
# vi-prev-word (not bound)
# vi-put (not bound)
# vi-redo (not bound)
# vi-replace (not bound)
# vi-rubout (not bound)
# vi-search (not bound)
# vi-search-again (not bound)
# vi-set-mark (not bound)
# vi-subst (not bound)
# vi-tilde-expand (not bound)
# vi-undo (not bound)
# vi-unix-word-rubout (not bound)
# vi-yank-arg (not bound)
# vi-yank-pop (not bound)
# vi-yank-to (not bound)
"\C-y": yank
"\e.": yank-last-arg
"\e_": yank-last-arg
"\e\C-y": yank-nth-arg
"\ey": yank-pop

```

## CH47-2 -- bind events on keys of keyboard
To bind an event on key of keyboard,

use `bind` built-in command (`ReadLine` library)

syntax:

```
bind '"{key}": "{value}"'
```

place `{key}` with the sequence code of keys 

and `{value}` as event.

> [!IMPORTANT]
> The `bind` takes the ONLY effect on this session.
>
> To take the effect on all session,
>
> please place command at startup file `.bashrc` or `~/inputrc` (`ReadLine` configuration file)
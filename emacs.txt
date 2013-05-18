;; A quick check that the .emacs is being read
;; (setq initial-buffer-choice t)
;; (insert "Hola Alvaro!")


;; disable splash screen
(custom-set-variables '(inhibit-startup-screen t))

;;; disable backup files 
(setq make-backup-files nil)

;; This  highlights all documents by default
(global-font-lock-mode t)


;; For Full Screen hitting F11.
(defun toggle-fullscreen (&optional f)
  (interactive)
  (let ((current-value (frame-parameter nil 'fullscreen)))
    (set-frame-parameter nil 'fullscreen
                         (if (equal 'fullboth current-value)
                             (if (boundp 'old-fullscreen) old-fullscreen nil)
                           (progn (setq old-fullscreen current-value)
                                  'fullboth)))))

(global-set-key [f11] 'toggle-fullscreen)

;; Make new frames fullscreen by default. Note: this hook doesn't do
;; anything to the initial frame if it's in your .emacs, since that file is
;; read _after_ the initial frame is created.
(add-hook 'after-make-frame-functions 'toggle-fullscreen)


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;  Second Party Packages ;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;  Load directory with packages ;;;;
;; this adds a new path to the load path ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; this dir is searched before the standard emacs ones ;;;;

(setq load-path (cons "~/Documents/code/emacs/modes" load-path))


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;; Load Particular Packages ;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;



;;;  Theme for the Red-Green Colorblind  ;;;
;; (require 'color-theme)
;; (color-theme-initialize)
;; (load-file "~/Documents/code/emacs/modes/themes/color-theme-colorblind.el")
;; (color-theme-colorblind)


(require 'color-theme)
(color-theme-initialize)
(load-file "~/Documents/code/emacs/modes/color-theme-6.6.0/themes/color-theme-railscasts.el")
(color-theme-railscasts)



;;;  like quicksilver for emacs ;;;
(require 'anything-config)   
(require 'anything-match-plugin)   
(global-set-key "\C-ca" 'anything)   
(global-set-key "\C-ce" 'anything-for-files)   





;;;   CSS mode    ;;;
; (require 'css-mode)
(autoload 'css-mode "css-mode" t)

(setq auto-mode-alist
      (append '(("\\.css$" . css-mode))
              auto-mode-alist))

(setq auto-mode-alist
      (append '(("\\.less$" . css-mode))
              auto-mode-alist))


;;;; to get proper indentation with html files we use nxml-mode
;; (setq auto-mode-alist
;;       (append '(("\\.html$" . nxml-mode))
;;               auto-mode-alist))







;;;   YASnippet is a template system for Emacs. It allows you to type an abbreviation and automatically expand it into function templates. Bundled language templates includes: C, C++, C#, Perl, Python, Ruby, SQL, LaTeX, HTML, CSS and more.
(require 'yasnippet-bundle)

;;;; github dot com/rooney/zencoding
;;;; Zencoding for html, css ;;;, works in combination with Yasnippet
(require 'zencoding-mode)
(add-hook 'sgml-mode-hook 'zencoding-mode) ;; Auto-start on any markup modes





;;;   display line number ;;;
(require 'linum)
(global-linum-mode 1)
(autoload 'linum "linum-mode" t)
(setq linum-format "%d ")




;;;;;;;;;;;;;;;;;;;;;;;;
;;;;    python-mode ;;;;
;;;;;;;;;;;;;;;;;;;;;;;;

(autoload 'python-mode "python-mode" "Python Mode." t) 
(add-to-list 'auto-mode-alist '("\\.py\\'" . python-mode)) 
(add-to-list 'interpreter-mode-alist '("python" . python-mode)) 

;;;;;  a python live debugger   ;;;;; 
(when (load "flymake" t) 
 (defun flymake-pyflakes-init () 
 (let* ((temp-file (flymake-init-create-temp-buffer-copy 
 'flymake-create-temp-inplace)) 
 (local-file (file-relative-name 
 temp-file 
 (file-name-directory buffer-file-name)))) 
 (list "pyflakes" (list local-file)))) 
 
 (add-to-list 'flymake-allowed-file-name-masks 
 '("\\.py\\'" flymake-pyflakes-init))) 


(defun my-compile ()
  "Use compile to run python programs"
  (interactive)
  (compile (concat "python " (buffer-name))))
(setq compilation-scroll-output t)

  (local-set-key "\C-c\C-c" 'my-compile)




;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;  WINDOWS MANAGEMENT  ;;;;;;;;;;;;;;;;;

(setq visible-bell t)
(display-time-mode 1)

;;;; simplify dired hiding details ;;;;


;; (require 'dired-details)
;; (dired-details-install)
;; (setq dired-listing-switches "-alh")

(add-hook 'dired-load-hook
            (function (lambda () (load "dired-x"))))

;; this below seems to be for dired-x, not installed yet.
(setq dired-omit-files
      (rx (or (seq bol (? ".") "#")         ;; emacs autosave files
              (seq bol "." (not (any "."))) ;; dot-files
              (seq "~" eol)                 ;; backup-files
              )))

; (add-hook 'dired-mode-hook (lambda () (dired-omit-mode 1))) 



;; current buffer name in title bar
(setq frame-title-format "%b")

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;  MOVING AROUND WINDOWS  ;;;;;;;;;;;;;


(global-set-key [M-left] 'windmove-left)          ; move to left windnow
(global-set-key [M-right] 'windmove-right)        ; move to right window
(global-set-key [M-up] 'windmove-up)              ; move to upper window
(global-set-key [M-down] 'windmove-down)          ; move to downer window





;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;; KEY BINDINGS ;;;;;;;;;;;;;;;;;;;;;;;

;; go to line ;; replaces set face;;

(global-set-key "\M-g" 'goto-line)

;; disable scrollbar and toolbar
(toggle-scroll-bar -1)
;(tool-bar-mode -1)

;; disable toolbar
(tool-bar-mode -1)


;; y n instead of yes no
(defalias 'yes-or-no-p 'y-or-n-p)


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Ido to smart do and open ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

  ;; Enable Ido
  (setq ido-enable-flex-matching t)
  (setq ido-everywhere t)
  (ido-mode 1)

  ;; when using Ido, guess the context
  ;;can be toggled with "ido-use-url-at-point"
 (setq ido-use-filename-at-point 'guess)

  ;; when new file is created, do so straight away
  ;; without asking for confirmation.
  (setq ido-create-new-buffer 'always)



;;;;;;;;;;;;;;;;;;;;
;; SSH with tramp ;;
;;;;;;;;;;;;;;;;;;;;

(setq tramp-default-method "ssh")


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; OPEN RECENT FILES with recentf and ido ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


(require 'recentf)
 
;; get rid of `find-file-read-only' and replace it with something
;; more useful.
(global-set-key (kbd "C-x C-r") 'ido-recentf-open)
 
;; enable recent files mode.
(recentf-mode t)
 
; 50 files ought to be enough.
(setq recentf-max-saved-items 50)
 
(defun ido-recentf-open ()
  "Use `ido-completing-read' to \\[find-file] a recent file"
  (interactive)
  (if (find-file (ido-completing-read "Find recent file: " recentf-list))
      (message "Opening file...")
    (message "Aborting")))

(require 'uniquify) ;; unique names for buffers
(setq uniquify-buffer-name-style 'post-forward) 


;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Custom Font and Faces ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;

(custom-set-faces
 '(default ((t (:inherit nil :stipple nil 
					;; :background "black" :foreground "white" 
			 ;; :Background "White" :Foreground "black" 
			 :Inverse-video nil :box nil :strike-through nil 
			 :overline nil :underline nil :slant normal :weight normal 
			 :height 110 :width normal :foundry "unknown" 
			 :family "DejaVu Sans Mono")))))

 ;; ;; 



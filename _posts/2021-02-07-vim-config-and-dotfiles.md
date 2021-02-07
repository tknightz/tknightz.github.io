---
title: Vim configs và dotfiles (.files)
excerpt_separator: "<!--more-->"
tags:
    - vim
    - dotfiles
categories:
    - realquick
comment: true
---
Long time ko viết blog, mình nhận ra viết mấy bài học thuật khá là tốn thời gian, viết mấy bài trên blog này mất bao nhiêu thời gian (một số bài chưa kịp post lên đây nhưng do cài mới Linux ko backup nên nó đi theo hđh cũ rồi). Hôm nay nghỉ Tết hơi rảnh nên post một bài real quick về **Vim** configs và dotfiles của mình.

<!--more-->


# **VIM**

## Real "SLOW" about journey to **VIM**

- Mình cũng mới dùng **Vim** thôi được gần 3 năm = thời gian dùng Linux. Do dòng đời xô đẩy nên "mới phải" dùng **Vim** (do còn máy tính cùi của mình ko dùng được **VSCode** -> **cấu hình máy : combo Intel i3 2xxx + 4GB + HDD (no SSD)**) mình cũng cảm thấy buồn vl khi phải chia tay **VSCode** cũng chỉ vì con chiến mã còi cọc của mình ko "thồ" được đồ nặng. Sau này khi có chiến mã mới khỏe mạnh nhưng vì quen mùi à nhầm lỡ yêu **Vim** mất rồi nên cũng chẳng màng nối lại tình xưa với **VSCode** nữa.

- Sau một thời gian dùng **Vim** mình cũng tự tạo được bộ config phục vụ công việc của mình *( nó đáp ứng hầu hết các yêu cầu của mình )*. Cái hay của **Vim** là nó có cả một ngôn ngữ lập trình đứng sau nó (**VimL** a.k.a **VimScript**) nên dường như bạn có thể làm được mọi thứ tùy vào yêu cầu của bạn. Nó cũng tương tự kiểu **Emacs** có nguyên một ngôn ngữ lập trình đằng sau nên nó làm được rất nhiều thứ. 

- Nếu ai hỏi **Emacs** với **Vim** thì cái nào làm được nhiều thứ hơn thì câu trả lời là.... tất nhiên là **Emacs** rồi. Thứ nhất vì **Emacs** là một phềm độc lập ko phụ thuộc vào phần mềm khác, thứ hai là **Emacs** là phần mềm GUI nên nó làm được nhiều cái mà **Vim** ko làm được như : xử lý liên quan đến hiển thị,...  (**Vim** làm việc trên terminal nên nó bị phụ thuộc vào những gì mà terminal hỗ trợ). **Emacs** làm được hầu hết mọi thứ : lướt web, nghe nhạc, ... bạn có thể chơi game trên **Emacs** luôn (nó ko phải là editor nữa mà là một hệ điều hành luôn rồi)

- Mình cũng dùng **Emacs** được một khoảng thời gian vì lúc đầu phân vân **Emacs** với **Vim** ko biết học cái nào nên mình học cả hai luôn, **Emacs** cũng có cái hay của nó nhưng mình thấy Org-mode của **Emacs** mới chính là **killer feature** đến giờ mình vẫn thỉnh thoảng dùng **Emacs** để note là chính do cái Org-mode ""xin xò" của nó.

- Vì dùng **Emacs** một thời gian **Vim** config của mình cũng bị ảnh hưởng bởi một số package của **Emacs** (which key, projectile,....). Sau khi về dùng **Vim** mình cũng cho ra đời một plugin tương tự với projectile của **Emacs** : [projectile.**vim**](https://github.com/tknightz/projectile.vim) - plugin mình viết khi học sau khoảng thời gian học **Vim**. Ngoài plugin này còn một số hàm mình viết bằng **VimL** trong configs của mình nữa.

- Ở đây là config của mình trên Nvim, với **Vim** hình như có một số plugin ko hỗ trợ.

## Cấu trúc các config của mình.

|Config|Note|
|-----|----|
|init.vim| Chứa các tên plugin được cài đặt,<br> bên dưới là load cái file config bên ngoài|
|mapping| Chứa các mapping - keybinding (nmap, vmap, ...)|
|function| Chứa một số function mình viết để giải quyết vấn đề của mình|
|variable| Chứa các biến được định nghĩa cho các plugin hoặc biến của **Vim**|
|which_key| Chứa config which_key thực chất là sắp xếp các keybinding của bạn để dùng với which_key|

-> Nhấn phím Space + s (setting) sẽ hiển thị ra các cài đặt:
{: .notice--warning}
![WhichKey Vim]({{'/images/realquick/vim/which_key_vim.png'}}){: .align-center}


## Một số Plugin nổi bật trong config **VIM** của mình

| **Plugin**                    | **Note**                                                   |
|---------------------------|--------------------------------------------------------|
| vim-airline/vim-airline | Hiện thị tab a.k.a buffers |
| sheerun/vim-polyglot    | Color syntax cho nhiều ngôn ngữ hơn          |
| liuchengxu/vim-which-key| Mình dùng sau khi sử dụng **Emacs** <br>( gợi ý phím tắt lúc khi bạn mới dùng **Vim** hoặc chưa quen với các phím tắt mới )<br> khi bạn đã quen với các phím tắt rồi thì hãy set nó ở chế độ lazy-mode cho nó nhanh.<br> Với cả dùng Space + phím làm mình thấy keybinding của mình có tổ chức hơn. |
|neoclide/coc.nvim| Autocompletion cho **Vim**|
|junegunn/fzf | |
|junegunn/fzf.vim|fuzzy tìm kiếm file nhanh chóng ( projectile.vim của mình cũng dựa trên cái này )|


Colorscheme trong config của mình là do mình tự tạo (chọn màu mè các thứ mycolo.vim)
{: .notice--success}


![Mycolo]({{'/images/realquick/vim/my_colo.png'}}){: .align-center}

# .dotfiles

Bài này là **real quick** nên mình cũng ko dài dòng nữa -> nói qua về bộ **dotfiles** của mình cũng ko có gì nổi bật chỉ lưu một số configurations của : zsh, tmux, nvim, vifm,.etc. Trong repo **dotfiles** của mình có vài files shell script mình viết để tự động cài đặt các thứ khi thay đổi hệ điều hành (**shellScript** viết trên **Manjaro** (**Arch**), có thể dùng trên **Ubuntu** nhưng mình lười chưa test bản mới nhất trên **Ubuntu**)

**Check it out** : [dotfiles](https://github.com/tknightz/dotfiles.git)
{: .notice--danger}

## 4 mins to read. It's real quick.
{: .notice--success}

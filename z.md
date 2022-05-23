
# تقوم الأداة z بتذكر المجلدات التي تتردد اليها بكثرة و تسهل عملية الوصول إليها
## الأداة سهلة و بسيطه و متوفرة علي جميع انظمة التشغيل و الصدفيات
## سنتناول اليوم طريقة تثبيتها و استعمالها علي لينكس

## نظرة علي الأداة  

```bash
[mo@test-lab ~]$ pwd
/home/mo
[mo@test-lab ~]$ cd /tmp/test/
[mo@test-labfolder_test]$ cd /home/mo/
[mo@test-lab ~]$ cd folder_test # لاحظ هنا عدم تعرف الأمر cd علي المجلد لانه غير موجود في نفس المكان 
bash: cd: zoxide: No such file or directory
[mo@test-lab ~]$ zfolder_test  #لنجرب الأنتقال باستخدام الأمر z 
[mo@test-lab folder_test]$ cd mo #نعود لمجلد المنزل
[mo@test-lab ~]$ z est # و نحاول تخمين الأسم و كتابة جزء منه 
[mo@test-lab folder_test]$ # تعرفت الأداة علي المجلد عن طريق خوارزمية التخمين
[mo@test-lab folder_test]$ z mo #مثال اخر لتذكر العنوان 
[mo@test-lab ~]$ # هناقامت الأداة بتذكر عنوان مجلد المنزل للحساب الخاص بي بسبب الإنتقال له من قبل  
```


## معاييرالخوارزميه 
### فهم معايير خوارزمية سيسهل علينا التعامل مع الأدة و توقع مخرجاتها 
### يحكم الأداة معايير المطابقه الثلاث و تكرار زيارة المجلدات بالأضافة لتوقيت اخر زيارة

#### ١ - لا تضع حالة الأحرف في الحسبان
``` z foo ==/foo ==/FOO لاحظ هنا ان الأداة تعرفت علي التعبرين و اهملت حالة الأحرف```
#### ٢ - تعتمد علي الترتيب 
``` z fo ba ==/foo/bar  !=/bar/foo لاحظ هنا عند عكس الترتيب لا تتعرف الاداة علي النمط ```
#### ٣ - تماثل الجزء الأخير
``` z bar ==/foo/bar !=/bar/foo لاحظ هنا ان bar هو الجزء الاخير في الامر لذلك تماثل الجزء الاول و لاتماثل الاخير ```

ملحوظة : 
== تعني أن z سيطابق المجلد و يتعرف علي اسم المجلد 
!= تعني أن z لن يطابق المجلد و سيقوم بتجاهله 

#### تعطي النتائج بشكل تنازلي بناء علي ترددك علي الملف مع مراعاة أولوية اعلي في حال كانت الزبارات في  الساعات الأخيرة 

## التثبيت
### ١ - تثبيت البرمجية علي نظامك
البرمجية موجوده علي غالبية المستودعات الرسمية قم بتثبيتها عن طريق مدير الحزم الخاص بنظامك

| التوزيعة           |المستودع                 | أمر التنزيل                                                                                   |
| ------------------ | ----------------------- | ---------------------------------------------------------------------------------------------- |
| Alpine Linux 3.13+ | [Alpine Linux Packages] | `apk add zoxide`                                                                               |
| Arch Linux         | [Arch Linux Community]  | `pacman -S zoxide`                                                                             |
| CentOS 7+          | [Copr]                  | `dnf copr enable atim/zoxide` <br /> `dnf install zoxide`                                      |
| Debian 11+         | [Debian Packages]       | `apt install zoxide`                                                                           |
| Devuan 4.0+        | [Devuan Packages]       | `apt install zoxide`                                                                           |
| Fedora 32+         | [Fedora Packages]       | `dnf install zoxide`                                                                           |
| Gentoo             | [GURU Overlay]          | `eselect repository enable guru` <br /> `emerge --sync guru` <br /> `emerge app-bashs/zoxide` |
| Manjaro            |                         | `pacman -S zoxide`                                                                             |
| NixOS              | [nixpkgs]               | `nix-env -iA nixpkgs.zoxide`                                                                   |
| Parrot OS          |                         | `apt install zoxide`                                                                           |
| Raspbian 11+       | [Raspbian Packages]     | `apt install zoxide`                                                                           |
| Ubuntu 21.04+      | [Ubuntu Packages]       | `apt install zoxide`                                                                           |
| Void Linux         | [Void Linux Packages]   | `xbps-install -S zoxide`                                                                       |

بإمكانك أيضا تثبيتها عن طريق الأنترنت عن طريق الأمر 

```sh
curl -sS https://webinstall.dev/zoxide | bash
```

### ٢-اضافة البرنامَج إلي الصدفية الخاصة بك 

لمعرفة الصدفية المستخدمة في نظامك تستطيع استخدام الأمر 
```sh 
echo $bash 
```


<details>
<summary>طرفية Bash</summary>
اضف إلي مِلَفّ الإعدادات الخاص بالصدفية (عادة يكون `~/.bashrc`):

```sh
eval "$(zoxide init bash)"
```
</details>

<details>
<summary>طرفية Fish</summary>
اضف إلي مِلَفّ الإعدادات الخاص بالصدفية (عادة يكون `~/.config/fish/config.fish`):

```fish
zoxide init fish | source
```
</details>

<details>
<summary>طرفية Zsh</summary>
اضف إلي مِلَفّ الإعدادات الخاص بالصدفية (عادة يكون `~/.zshrc`):

```sh
eval "$(zoxide init zsh)"
```
For completions to work, the above line must be added *after* `compinit` is
called. You may have to rebuild your cache by running
`rm ~/.zcompdump*; compinit`.
</details>

<details>

<summary>أي صدفية تتوافق مع معايير posix</summary>
اضف إلي مِلَفّ الإعدادات الخاص بالصدفية:

```sh
eval "$(zoxide init posix --hook prompt)"
```
</details>

مثال علي صدفية bash

```bash

[mo@test-lab ]$ which $bash
/bin/bash
[mo@test-lab ~]$ # الصدفية الخاصة بنا هي bash 
[mo@test-lab ~]$ # سنقوم بإضافة الأداة الي ملف الصدفة الخاصة بنا )حسب ملف صدفيتك(
[mo@test-lab ~]$ echo 'eval "$(zoxide init bash)"' >> ~/.bashrc
[mo@mo-lab ~]$ source ~/.bashrc # نقوم بإعادة تحميل ملف الإعدادات حتي تظهر التغيرات 
[mo@test-lab ~]$ z
[mo@test-lab ~]$ z /home
[mo@test-lab home]$
```


### ٣-خطوة اختيارية قم بتحميل fzf 
[fzf](https://github.com/junegunn/fzf) هو برنامَج مطابقة غير دقيقة سيسهل علينا عملية الاختيار  في حال كان هناك اكثر من بديل لنفس المجلد المطلوب 
قم بالتحميل عن طريق مدير الحزم الخاص بك 

|    مدير الحزم   |      التوزيعة           |    الأمر                            |
| ---             | ---                     | ---                                |
| APK             | Alpine Linux            | `sudo apk add fzf`                 |
| APT             | Debian 9+/Ubuntu 19.10+ | `sudo apt-get install fzf`         |
| Conda           |                         | `conda install -c conda-forge fzf` |
| DNF             | Fedora                  | `sudo dnf install fzf`             |
| Nix             | NixOS, etc.             | `nix-env -iA nixpkgs.fzf`          |
| Pacman          | Arch Linux              | `sudo pacman -S fzf`               |
| pkg             | FreeBSD                 | `pkg install fzf`                  |
| pkgin           | NetBSD                  | `pkgin install fzf`                |
| pkg_add         | OpenBSD                 | `pkg_add fzf`                      |
| XBPS            | Void Linux              | `sudo xbps-install -S fzf`         |
| Zypper          | openSUSE                | `sudo zypper install fzf`          |

ملاحظة : zoxide  يدعم الإصدار  0.21.0 و أعلي  

### ٤-خطوة اختيارية استيراد البيانات الخاصة بك 

<details>
<summary>autojump</summary>

```sh
zoxide import --from autojump path/to/db
```

</details>


<details>

<summary>z, z.lua, or zsh-z</summary>

```sh
zoxide import --from z path/to/db
```

</details>

## الإعداد 
### استبدال cd بz 
خطوة اختيارية لمن لا يفضل كتابة الأمر z و يبقي مع الأفتراضيات
بداخل ملف اعدادات الصدفية الذي قمنا بتعديله سابقا قم بإضافة ```alias cd='z' ```  
``` sbash
[mo@mo-lab ~]$ cd test # تجربة الإتنقال الي مجلد بستخدام cd 
bash: cd: test: No such file or directory
[mo@mo-lab ~]$ z test # تجربة مرة اخر باستخدام z بعد عدم تعرف cd 
[mo@mo-lab test]$ pwd # انتقل z الي المجلد بنجاح 
/tmp/test
[mo@mo-lab test]$ cd ~ # سنعود الي مجلد المنزل 
[mo@mo-lab ~]$ echo "alias cd='z'" >> ~/.bashrc سنقوم بإعادة تعريف الأمر cd  و نجعل الصدفية تنفذ الأمر z عند كتابة cd 
[mo@mo-lab ~]$ source ~/.bashrc # نقوم بإعادة تحميل ملف الإعدادات حتي تظهر التغيرات 
[mo@mo-lab ~]$ cd test
[mo@mo-lab test]$ pwd # انتقل بنجاح لأن ما ينفذ في اخلفية هو الأمر z 
/tmp/test
[mo@mo-lab test]$ #و هكذا قمنا بتحويل الأمر cd  و بإمكانا استعمال cd و سنحصل علي مميزات z  
```
### تغيير طريقة عمل z وجعلها تطبع المجلد بعد الأنتقال 
خيار حسب التفضيل هناك من يحب ان يحصل علي رد فعل مرأي يووضح المجلد الذي تم الأنتقال اليه
بداخل ملف اعدادات الصدفية الذي قمنا بتعديله سابقا قم بإضافة ```export _ZO_ECHO=1``` 
``` bash 
[mo@mo-lab ~]$ z test
/tmp/test # في حال تفعيل الخيار سيقوم بطباعة عنوان الملف بعد الإنتقال 
[mo@mo-lab test]$ 
``` 
## رأي الشخصي في الأداة 
هل تقوم الأداة باستبدال الأمر cd نظريا نعم فهي تقوم بكل ما يقوم به و مكتوبة بلغة rust مما يجعلها مفضلة من البعض لسرعتها 
ولكن لن يكون انتقال المجتمع بأكمله و جعلها الخِيار الإفتراضي بتلك السهولة قم بتحمليها و اخبرني برأيك في التعليقات    

## المصادر 
https://github.com/ajeetdsouza/zoxide/
https://github.com/junegunn/fzf
##ترخيص

هذا الموضوع يتبع ترخيص جميع مواضيع أسس هو CC-BY-SA 4.0


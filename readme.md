Коміт в Git репозиторії зберігає моментальну копію всіх файлів в поточній директорії.

Git також зберігає історію коли і ким був створений той чи інший коміт. Тому більшість комітів мають комітів-предків, що знаходяться вище в ієрархії. Історія — це необхідна річ для кожного, хто працює з конкретним проектом.

(git commit)


Гілки - це просто посиланнями на конкретний коміт, нічого більше.
роби гілки завчасно, роби гілки часто

Через те, що сворення нових гілок ніяк не впливає на використання пам’яті чи дискового простору, набагато простіше розділити свою роботу на кілька логічно зв’язаних по функціоналу гілок, ніж працювати з величезними гілками.

гілка просто зберігає роботу теперішнього коміту і всіх його попередників.

(git branch newImage)

Гілка newImage посилається на коміт С1


якщо ми захочемо закомітити ці зміни, то main просунеться далі на нову гілку, а newImage залишиться, тому що ми були не на новій гілці.
Щоб перекинути зміни на нову гілку ДО коміту треба 

git checkout - щоб повернутись до гілки (*)

(git checkout newImage; git commit)



Злиття гілок дозволить поєднувати інфу з двох чи більше гілок. 
Це дозволить нам відгілкуватись, зробити нову фічу, й потім інтегрувати її назад. 

Перший спосіб об’єднувати робочу інфу з яким ми розберемось це git merge

Команда merge (злити) в Git створює спеціальний коміт який має двох унікальних батьків. Коміт з двома батьками в принципі просто значить що в нього включена інфа з обох батьків і всіх їх попередників



Дві гілки, з двома окремими комітами. Жодна з них не містить повної робочої інформації



змерджимо bugFix в main


(git merge bugFix)


main тепер вказує на коміт з двома батьками. Якщо ти піднімешся вверх з цього коміту по дереву, починаючи з main, на шляху ти зустрінеш кожен коміт аж до кореневого. Це означає що гілка main тепер містить всю інфу в цьому репозиторії. 

Кожен бранч виділено окремим кольором. Колір кожного коміту це суміш кольорів всіх гілок що містять цей коміт. Тож ми бачимо що колір гілки main містять всі коміти, але не колір bugFix


змерджимо main в bugFix

(git checkout bugFix; git merge main)




Так як bugFix є нащадком main, git'у не потрібно нічого робити; він просто пересунув bugFix на той самий коміт, на якому знаходиться main. 

Тепер всі коміти одного кольору, що означає що кожен бранч включає в собі всю корисну інфу яка є в цьому репозиторії!



Git Rebase

Інший спосіб комбінування змін з різних бранчів називається rebase. Rebase по суті бере кілька комітів , "копіює" їх, й кладе їх в інше місце. 
Основна перевага rebase в тому, що його використовують щоб створити зручну лінійну послідовність комітів. Коміт лог та історія будуть виглядати набагато чистіша, якщо користуватися лише rebase (а не merge)


Ми знову маємо дві гілки; зауваж, що наразі вибрана гілка bugFix (вважай зірочку).

Ми хочемо перемістити наші зміни з гілки bugFix прямо до змін з гілки main. Тоді це буде виглядати наче ці зміни були додані одна за одною, хоча насправді вони були додані одночасно.



(git rebase main)



Тепер зміни з гілки bugFix знаходяться прямо попереду змін з main й ми отримали зручну лінійну послідовність комітів. Вважай що коміт C3 досі десь існує (в дереві він тьмяніший за решту), й C3' це "копія" яку ми заребейсили в main. 

Є лише одна проблема: гілка main також не була оновлена



Тепер ми перейшли (checkout) до гілки main. Далі робимо rebase на bugFix


(git rebase bugFix)



Так як main це предок bugFix, git просто просунув посилання гілки main вперед в історії.

روش Resolution یکی از مهم‌ترین تکنیک‌های استنتاج در هوش مصنوعی و سیستم‌های خبره است که برای اثبات قضایا در منطق محمولات (Predicate Logic) استفاده می‌شه.

## مفهوم کلی Resolution

Resolution 
یه روش proof by contradiction هست که برای اثبات درستی یک قضیه، فرض می‌کنه که قضیه غلط است و سعی می‌کنه به تناقض برسه. اگه تناقض پیدا کرد، یعنی قضیه اصلی درست بوده.

## مراحل کار Resolution

### 1. تبدیل به CNF (Conjunctive Normal Form)

ابتدا باید همه‌ی عبارات منطقی رو به فرم CNF تبدیل کرد:

- حذف Implication: `P → Q` به `¬P ∨ Q` تبدیل می‌شه
- اعمال قوانین De Morgan
- توزیع OR روی AND

### 2. اضافه کردن نقیض هدف

اگه می‌خوایم ثابت کنیم که `G` درست است، `¬G` رو به مجموعه clause ها اضافه می‌کنیم.

### 3. اعمال Resolution Rule

قانون اصلی Resolution:

```
از دو clause: (P ∨ A) و (¬P ∨ B)
نتیجه: (A ∨ B)
```

### 4. ادامه تا رسیدن به تناقض

این فرآیند رو ادامه می‌دیم تا به empty clause (□) برسیم که نشان‌دهنده‌ی تناقض است.

## مثال عملی

فرض کن می‌خوایم ثابت کنیم: "سقراط فانی است"

**پیش‌فرض‌ها:**

1. همه‌ی انسان‌ها فانی هستند: `∀x (Human(x) → Mortal(x))`
2. سقراط انسان است: `Human(Socrates)`

**هدف:** `Mortal(Socrates)`

**مراحل Resolution:**

1. تبدیل به CNF:
    
    - `¬Human(x) ∨ Mortal(x)`
    - `Human(Socrates)`
2. اضافه کردن نقیض هدف:
    
    - `¬Mortal(Socrates)`
3. اعمال Resolution:
    
    ```
    ¬Human(x) ∨ Mortal(x)    [x = Socrates]
    ¬Mortal(Socrates)
    ________________________
    ¬Human(Socrates)
    
    Human(Socrates)
    ¬Human(Socrates)
    ________________
    □ (Empty Clause)
    ```
    
4. رسیدن به تناقض نشان می‌ده که `Mortal(Socrates)` درست است.
    

## مزایا و کاربردها

- **کامل (Complete):** اگه یه قضیه قابل اثبات باشه، Resolution حتماً اون رو پیدا می‌کنه
- **صحیح (Sound):** هر چیزی که Resolution اثبات کنه، منطقاً درست است
- **مکانیزه:** به راحتی قابل پیاده‌سازی در کامپیوتر

## پیاده‌سازی در سیستم‌های خبره

در سیستم‌های خبره، Resolution برای:

- استنتاج حقایق جدید از knowledge base
- پاسخ به سوالات کاربر
- تشخیص تناقضات در دانش

استفاده می‌شه. معمولاً با الگوریتم‌هایی مثل SLD-Resolution (برای Prolog) پیاده‌سازی می‌شه.

این روش پایه و اساس بسیاری از سیستم‌های استنتاج منطقی محسوب می‌شه و توی زبان‌هایی مثل Prolog به صورت بنیادی استفاده می‌شه.


-----------------------------------------------------------------

# روش Resolution در هوش مصنوعی

## تعریف و مفهوم اصلی

Resolution یک **الگوریتم استنتاج کامل** برای منطق محمولات (First-Order Logic) است که توسط Robinson در سال 1965 معرفی شد. این روش بر اساس **proof by contradiction** کار می‌کند.

## اصول بنیادی

### اصل Resolution

```
اگر دو clause داشته باشیم:
C1: L1 ∨ L2 ∨ ... ∨ Ln ∨ P
C2: M1 ∨ M2 ∨ ... ∨ Mm ∨ ¬P

آنگاه می‌توانیم resolvent بسازیم:
R: L1 ∨ L2 ∨ ... ∨ Ln ∨ M1 ∨ M2 ∨ ... ∨ Mm
```

### مفاهیم کلیدی

**Clause:** یک disjunction از literal ها

- مثال: `P(x) ∨ ¬Q(x) ∨ R(a)`

**Literal:** یک atomic formula یا نقیض آن

- مثال: `P(x)` یا `¬P(x)`

**Resolvent:** نتیجه‌ای که از resolution دو clause بدست می‌آید

**Unification:** فرآیند پیدا کردن substitution برای متغیرها

## الگوریتم کامل Resolution

### مرحله 1: تبدیل به CNF

همه formulas باید به Conjunctive Normal Form تبدیل شوند:

1. **حذف Implication و Biconditional:**
    
    - `P → Q` تبدیل به `¬P ∨ Q`
    - `P ↔ Q` تبدیل به `(P → Q) ∧ (Q → P)`
2. **حرکت نقیض به داخل (De Morgan):**
    
    - `¬(P ∧ Q)` تبدیل به `¬P ∨ ¬Q`
    - `¬(P ∨ Q)` تبدیل به `¬P ∧ ¬Q`
3. **حذف Quantifiers:**
    
    - **Skolemization** برای existential quantifiers
    - **Universal instantiation** برای universal quantifiers
4. **توزیع OR روی AND:**
    
    - `P ∨ (Q ∧ R)` تبدیل به `(P ∨ Q) ∧ (P ∨ R)`

### مرحله 2: ساخت مجموعه Clauses

```
Knowledge Base: KB = {C1, C2, ..., Cn}
Goal: G
Query Set: S = KB ∪ {¬G}
```

### مرحله 3: اعمال Resolution Algorithm

```pseudocode
function Resolution(S):
    new = {}
    repeat:
        for each pair (Ci, Cj) in S:
            resolvent = Resolve(Ci, Cj)
            if resolvent == □:  // Empty clause
                return "Theorem proved"
            new = new ∪ {resolvent}
        
        if new ⊆ S:
            return "Cannot prove"
        
        S = S ∪ new
```

## مثال کامل: مسئله Kings و Crimes

**Knowledge Base:**

1. `American(x) ∧ Weapon(y) ∧ Sells(x,y,z) ∧ Hostile(z) → Criminal(x)`
2. `Owns(Nono, M1)`
3. `Missile(M1)`
4. `American(West)`
5. `Missile(x) ∧ Owns(Nono,x) → Sells(West,x,Nono)`
6. `Missile(x) → Weapon(x)`
7. `Enemy(x,America) → Hostile(x)`
8. `Enemy(Nono,America)`

**هدف:** اثبات `Criminal(West)`

### تبدیل به CNF:

1. `¬American(x) ∨ ¬Weapon(y) ∨ ¬Sells(x,y,z) ∨ ¬Hostile(z) ∨ Criminal(x)`
2. `Owns(Nono, M1)`
3. `Missile(M1)`
4. `American(West)`
5. `¬Missile(x) ∨ ¬Owns(Nono,x) ∨ Sells(West,x,Nono)`
6. `¬Missile(x) ∨ Weapon(x)`
7. `¬Enemy(x,America) ∨ Hostile(x)`
8. `Enemy(Nono,America)`
9. `¬Criminal(West)` (نقیض هدف)

### مراحل Resolution:

**گام 1:** از (3) و (6) با x=M1:

```
Missile(M1) + ¬Missile(x) ∨ Weapon(x)
→ Weapon(M1)
```

**گام 2:** از (2), (3) و (5) با x=M1:

```
Owns(Nono,M1) + Missile(M1) + ¬Missile(x) ∨ ¬Owns(Nono,x) ∨ Sells(West,x,Nono)
→ Sells(West,M1,Nono)
```

**گام 3:** از (8) و (7) با x=Nono:

```
Enemy(Nono,America) + ¬Enemy(x,America) ∨ Hostile(x)
→ Hostile(Nono)
```

**گام 4:** از (1), (4), Weapon(M1), Sells(West,M1,Nono), Hostile(Nono):

```
¬American(x) ∨ ¬Weapon(y) ∨ ¬Sells(x,y,z) ∨ ¬Hostile(z) ∨ Criminal(x)
با x=West, y=M1, z=Nono
→ Criminal(West)
```

**گام 5:** با (9):

```
Criminal(West) + ¬Criminal(West)
→ □ (Empty Clause)
```

**نتیجه:** تناقض رسیدیم، پس `Criminal(West)` اثبات شد.

## نکات مهم و کاربردها

### مزایا:

- **Completeness:** اگر theorem قابل اثبات باشد، حتماً پیدا می‌کند
- **Soundness:** هر چیزی که اثبات کند، منطقاً صحیح است
- **Mechanical:** کاملاً قابل برنامه‌نویسی
- **General:** برای تمام منطق محمولات کار می‌کند

### محدودیت‌ها:

- **Undecidable:** ممکن است تا ابد ادامه پیدا کند
- **Exponential complexity:** در بدترین حالت
- **Memory intensive:** تعداد clauses به سرعت رشد می‌کند

### کاربردها در سیستم‌های خبره:

- **Theorem proving**
- **Logic programming** (Prolog)
- **Expert systems inference**
- **Automated reasoning**
- **Knowledge base consistency checking**

### بهینه‌سازی‌ها:

- **Set of Support Strategy**
- **Unit Resolution**
- **Linear Resolution**
- **SLD Resolution** (برای Horn clauses)

این روش پایه و اساس بسیاری از سیستم‌های استدلال منطقی مدرن است و در زبان‌هایی مثل Prolog به صورت بنیادی پیاده‌سازی شده.


-----------------------------------------------

بیا این مثال رو قدم به قدم حل کنیم:

## داده‌ها:

```
KB = (B₁,₁ ↔ (P₁,₂ ∨ P₂,₁)) ∧ (¬B₁,₁)
Query: ¬P₁,₂
```

## حل با Resolution:

### مرحله 1: تبدیل KB به CNF

**تبدیل biconditional:**

```
B₁,₁ ↔ (P₁,₂ ∨ P₂,₁) 
≡ (B₁,₁ → (P₁,₂ ∨ P₂,₁)) ∧ ((P₁,₂ ∨ P₂,₁) → B₁,₁)
≡ (¬B₁,₁ ∨ P₁,₂ ∨ P₂,₁) ∧ (¬P₁,₂ ∧ ¬P₂,₁ ∨ B₁,₁)
≡ (¬B₁,₁ ∨ P₁,₂ ∨ P₂,₁) ∧ ((¬P₁,₂ ∨ B₁,₁) ∧ (¬P₂,₁ ∨ B₁,₁))
```

**مجموعه clauses:**

1. `¬B₁,₁ ∨ P₁,₂ ∨ P₂,₁`
2. `¬P₁,₂`
3. `¬P₂,₁ ∨ B₁,₁`

### مرحله 2: اضافه کردن نقیض query

5. `P₁,₂` (نقیض ¬P₁,₂)

### مرحله 3: اعمال Resolution

**Resolution بین (4) و (2):**

```
¬B₁,₁  +  ¬P₁,₂ ∨ B₁,₁
→ ¬P₁,₂
```

**Resolution بین نتیجه و (5):**

```
¬P₁,₂  +  P₁,₂
→ □ (Empty Clause)
```

## نتیجه:

تناقض رسیدیم، پس **¬P₁,₂ قابل استنتاج است**.

## تفسیر منطقی:

- از `¬B₁,₁` و `¬P₁,₂ ∨ B₁,₁` نتیجه می‌گیریم که `¬P₁,₂`
- یعنی در خانه (1,2) حفره نیست

بله، `¬P₁,₂` از KB قابل استنتاج است.

---
layout: post
title: Lilypond 手冊——常用標記
categories: [Lilypond Manuals]
excerpt: Lilypond Manuals - Common Notation
---
* Index
{:toc}

提示（省略的材料）：之後的內容中，大多數的示例將審略`{...}`等語法，這是因為如此你便可以直接插入大多數的示例到正在編輯的文件之中，但這並不表明你在使用的時候不需要這些語法。同樣的，請記住添加版本聲明`\version`。

## 獨立五線譜標記

### 小節線與雙縱線

	% 使用 \bar 表示小節線
	g1 e1 \bar "||" c2. c'4 \bar "|."

	% 使用 | 執行強制檢查
	g1 | e1 | c2. c'4 | g4 c g e | c4 r r2 |

**要點：**

- 無特別指明下，小節線會自動添加
- `||`表示雙縱線，`|`表示結束線
- 職行強制檢查後，程式會自動調整輸入的音符符合當前小節的時值，且這會讓你的源碼更規整易讀

### 變音記號

	% 使用 is(isis) 或 es(eses) 聲明升（重升）或降（重降）
	% 如有必要，也可以使用 ! 來聲明提示性變音記號
	cis4 ees fisis , aeses

	% 使用 \key \major(\minor) 聲明調號與調式
	\key d \major
	a1 |
	\key c \minor
	a1 |

**注意：**

為了明確是否顯示變音記號，LilyPond 會檢查音高與調號。調號僅僅會影響**顯示的**變音記號，而不是音符的實際**音高**。這經常會造成混亂，以下會詳細說明。

LilyPond 會嚴格區分樂曲內容與排版。音符升（降，還原）是音高的一部分，這屬於樂曲內容，而是否在相關音符前顯示變音記號則是排版的問題，排版設計遵守一定的規則，基於這些規則程式會自動確定是否顯示變音記號。音高則是藝術的一部分，所以必要情況下必須特別聲明以確定你聽到的實際音高。

例：
	\key d \major
	cis4 d e fis

在這個例子中，音符上沒有顯示任何的變音記號，但是卻必須添加`is`在源碼之中。

`b`並不表示「在五線譜的中線上搞個黑點」而是說明「這個位置有一個實際音高為 B 的音符」，所以在降 A 大調中，會顯示還原符號。

	\key aes \major
	aes4 c b c

如果以上內容看得很混亂，那麼只需記住：如果你想要表示的音符存在於鍵盤的黑鍵上，那麼你就必需特別聲明。

如何正確的鍵入聲明可能會需要稍稍付出一點努力，但好處卻是使得移調更加的方便，變音記號在不同的慣例下可能會有區別，詳細請參閱「Notation Reference」。

### 同音連線與連線

	% 使用 ~ 表示同音音線
	g4~ g c2~ | c4~ c8 a~ a2 |

	% 使用 () 表示連線
	d4 ( c16) cis ( d e c cis d) e ( d4)

	% 使用 \( \) 劃分樂句，可以與連線同時存在但不可以同時使用
	g4 \( g8 ( a) b ( c) b4\)

**要點：**

- 同音連線只能用於相同音高的音來增加音符時值
- 連線可以用於表達一組音

### 演奏法與力度標記

	% 使用 -* 表示通常的演奏法標記
	c4-^ c-+ c-- c-!
	c4-> c-. c2-_

	% 使用 -num 表示指法
	c4-3 e-5 b-2 a-1

	% 使用 ^（上）或 _（下）替代 - 來聲明演奏法或指法的顯示位置
	c4_-^1 d^. f^4_2-> e^-_+

	% 使用 \* 表示力度
	c4\< c\ff\\> c c\!

### 添加註解

	 % 聲明顯示位置的用法同於演奏法
	c2^"espr" a_"legato"

	% 使用 \markup 聲明額外的樣式
	c2^\markup { \bold espr }
	a2_\markup {
		\dynamic f \italic \small { 2nd } \hspace #0.1 \dynamic p
	}

### 設定符杠

	% 使用 [] 手動設置符杠（默認會自動設定）
	a8 [ ais] d [ ees r d] c16 b a8

	% 使用 \autoBeamOff 關閉自動設定符杠
	\autoBeamOff
	a8 c b4 d8. c16 b4 |
	\autoBeamOn
	a8 c b4 d8. c16 b4 |

### 高級節奏

#### 弱起小節

	% 使用 \partial num 聲明弱起小節
	\partial 8 f8 |
	c2 d |

#### 連音

	% 使用 \tuplet fraction 表示連音，分數的含義是用 */ 個音來表示 /* 個音
	\tuplet 3/2 { f8 g a }
	\tuplet 3/2 { c8 r c }
	\tuplet 3/2 { f,8 g16 [  a g a] }
	\tuplet 3/2 { d4 a8 }

#### 裝飾音

	% 使用 \grace 聲明裝飾音，使用 \appoggiatura 或 \acciaccatura 增添修辭
	c2 \grace { a32 b } c2 |
	c2 \appoggiatura b16 c2 |
	c2 \acciaccatura b16 c2 |

## 多聲部

:: LilyPond 中，音樂的表達同數學表達有異曲同工之妙。::

### 多旋律同時進行

	% 使用 <<>> 表示同時進行的多條旋律
	\relative c'' {
		<<
			{ a2 g }
			{ f2 e }
			{ d2 b }
		>>
	}

	% 在單譜表上表示多個音符
	% LilyPond 通過 <<>> 中開頭的表達來確定多條旋律亦或多個音符
	\relative c'' {
	c2 << c e >> |
	<< { e2 f } { c2 << b d >> } >> |
	}

### 多譜表

	% 使用 \new Staff 新增一張譜表
	\relative c'' {
		<<
			\new Staff { \clef "treble" c4 }
			\new Staff { \clef "bass" c,,4 }
		>>
	}

**注意：**

拍號會對所有的譜表起作用，而調號則只對當前譜表起作用。這是因為不同的樂器間變調比變換拍號更常見。

例：

	\relative c'' {
		<<
			\new Staff { \clef ''treble'' \key d \major \time 3/4 c4 }
			\new Staff { \clef ''bass'' c,,4 }
		>>
	}

### 連譜表

	% 使用 \new PianoStaff 表示鋼琴大譜表
	\relative c'' {
		\new PianoStaff <<
			\new Staff { \time 2/4 c4 e | g g, | }
			\new Staff { \clef ''bass'' c,,4 c' | e c | }
		>>
	}

**要點：**

- 使用 \\new GrandStaff 適配管弦樂隊總譜
- 使用 \\ new ChoirStaff 適配合唱總譜

###  組合音符為和弦

	% 使用 <> 來表示一組和弦
	r4 < c e g > < c f a >2

**要點：**

- 和弦內音的時值必須相等，表示一組和弦的時值應將標記放在 \<\> 之後
- 將一組和弦考慮為單音，這就是說單音能夠使用的標記也可以對一組和弦使用

## 歌曲

### 簡單歌曲的填詞

	% 使用 \addlyrics 填詞
	% <<>> 聲明樂曲和詞同時出現
	<<
	\relative c'' {
		\key g \major
		\time 6/8
		d4 b8 c4 a8 | d4 b8 g4
	}
	\addlyrics {
	Girls and boys come | out to play ,
	}
	>>


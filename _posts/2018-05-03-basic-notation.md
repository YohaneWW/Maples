---
layout: post
title: Lilypond 手冊——基本標記
categories: [Lilypond Manuals]
excerpt: Lilypond Manuals - Basic Notation
---
* Index
{:toc}

## 音高

	% 使用 \Relative 來聲明中央 C（默認使用高音譜號）
	\relative c' {
		c d e f
		g a b c
	}

**要點：**

- Relative 模式的邏輯是使首音符以就近原則接近定義的中央 C
- 如無特別聲明，之後的音將以就近原則接近**前面**一個音
- 可以通過添加（或刪除）`'`或者`,`來改變八度位置

## 時長（節奏）

	% 使用羅馬數字來聲明時長（默認使用四分音符與四四拍）
	\relative c'' {
		a1
		a2 a4 a8 a
		a16 a a a a32 a a a a64 a a a a a a a a2  
	}

**要點：**

- `1`表示全音符，`2`表示二分音符，`4`表示四分音符...（連線將自動添加）
- 如無特別聲明，之後的音將重複**前面**一個音的時長
- 可以使用`.`來表示附點，表示附點的音符必須聲明時長

## 休止

	% 使用 r 來表示休止
	\relative c'' {
		a4 r r2
		r8 a r4 r4. r8
	}

## 拍號

	% 使用 \time 來聲明拍號
	\relative c'' {
		\time 3/4
		a4 a a
		\time 6/8
		a4. a
		\time 4/4
		a4 a a a
	}

## 速度

	% 使用 \tempo 來聲明速度與節奏
	\relative c'' {
		\time 3/4
		\tempo "Andante"
		a4 a a
		\time 6/8
		\tempo 4. = 96
		a4. a
		\time 4/4
		\tempo  "Presto" 4 = 120
		a4 a a a
	}

## 譜號

	% 使用 \clef 來聲明譜號
	\relative c' {
		\clef "treble"
		c1
		\clef "alto"
		c1
		\clef "tenor"
		c1
		\clef "bass"
		c1
	}

## Working on input files

- 版本聲明：每個 LilyPond 文件中都必須加入類似`\version "2.18.2"`來聲明版本，按照慣例一般寫在開頭，若省略版本聲明，編譯時會發生錯誤。

這樣做有兩個好處，一是使 LilyPond 在版本迭代中自動更新編譯所需的語法，二是它可以告知 LilyPond 如何編譯這個文件。

- 區分大小寫：陳述部分如非必要請使用小寫以免發生錯誤
- 不限定空格：確定有無而無關數量
- 陳述：使用`{}`來表示需要編譯的陳述片段
- 註釋：使用`%`或`% {}`來表示不需要編譯的註釋

# HackScan — AI 게임 핵 감지 도구 / AI Game Hack Detector / AI ゲームチート検出ツール

**언어 / Language / 言語**

🇰🇷 [한국어](#-한국어) · 🇺🇸 [English](#-english) · 🇯🇵 [日本語](#-日本語)

---
## 🚀 바로 사용하기 (설치 없음)

아래 링크로 바로 접속해서 사용할 수 있어요. 아무것도 설치 안 해도 됩니다.

```
https://bigad2007.github.io/HackScan/
```
---

# 🇰🇷 한국어

> ⚠️ **교육 목적 프로젝트입니다.**
> 이 프로젝트는 웹 개발, AI API 연동, 클라이언트 사이드 영상 처리 등의 학습을 위한 교육용 예제입니다.
> 실제 핵 탐지 목적으로 사용하거나, 타인을 근거 없이 신고하는 용도로 사용하지 마십시오.
> 분석 결과는 AI의 추론이며 어떠한 법적 효력도 없습니다.

---

## 📖 프로젝트 개요

HackScan은 **단일 HTML 파일**로 구성된 클라이언트 사이드 웹 애플리케이션입니다.
사용자가 게임 영상을 업로드하고 Groq API 키를 입력하면, Llama AI가 해당 영상의 프레임과 메타데이터를 분석해 핵 사용 의심 여부를 JSON 형태로 반환하고, 결과를 시각적으로 렌더링합니다.

서버가 없으며 모든 처리는 브라우저에서 이루어집니다.

---

## 🎯 학습 목표

이 프로젝트를 통해 다음 개념을 실습할 수 있습니다.

| 분야 | 내용 |
|------|------|
| **AI API 연동** | Groq API (`/openai/v1/chat/completions`) 직접 호출, 브라우저-to-API 인증, JSON 프롬프트 엔지니어링 |
| **영상 처리** | `<video>` 요소로 Blob URL 생성, Canvas를 이용한 프레임 캡처, `seeked` 이벤트 기반 비동기 처리 |
| **DOM 조작** | 순수 JavaScript로 동적 UI 렌더링, 이벤트 위임, classList 조작 |
| **보안 기초** | XSS 방지를 위한 `escHtml()`, 외부 입력값 화이트리스트 검증, CSS 클래스 인젝션 방지 |
| **UX 패턴** | 프로그레스 애니메이션, 토스트 알림, 드래그앤드롭 업로드, 반응형 레이아웃 |
| **메모리 관리** | `URL.revokeObjectURL()`로 Blob URL 해제, 이벤트 리스너 중복 등록 방지 |

---

## 🗂️ 파일 구조

```
index.html          # 전체 애플리케이션 (CSS + HTML + JS 단일 파일)
README.md           # 이 문서
```

---

## 🚀 실행 방법

별도 빌드나 서버 없이 `index.html`을 브라우저에서 바로 열면 됩니다.

```bash
# 로컬에서 열기
open index.html

# 또는 간단한 로컬 서버 사용 (권장)
python3 -m http.server 8080
# 브라우저에서 http://localhost:8080 접속
```

### 사용 순서

1. 게임 종류 선택 (발로란트, 배그, 오버워치 등 6종)
2. 감지 민감도 / 시점 / 분석 깊이 설정
3. 게임 영상 업로드 (MP4, AVI, MOV, MKV, WebM / 최대 2GB)
4. [Groq Console](https://console.groq.com/keys)에서 발급한 API 키 입력
5. **분석 시작** 클릭

---

## 🔑 API 키 관련

- API 키는 브라우저에서 Groq 서버로 **직접** 전송됩니다.
- 이 애플리케이션의 서버(또는 제3자)에는 **전혀 저장되지 않습니다.**
- 단, 브라우저 개발자 도구 네트워크 탭에서 요청 헤더가 노출될 수 있으므로, 공용 PC에서는 사용을 권장하지 않습니다.
- Groq는 현재 무료 티어를 제공하고 있으나, API 사용량에 따라 요금이 부과될 수 있습니다.

---

## ⚙️ 주요 코드 구조

```
index.html
├── <style>          CSS 변수 시스템, 컴포넌트별 스타일, 반응형 미디어쿼리
├── <body>
│   ├── <nav>        상단 고정 네비게이션
│   ├── .hero        히어로 섹션 (데모 수치 표시)
│   └── .main
│       ├── 업로드 패널    게임 선택 / 분석 설정 / 파일 업로드 / API 키
│       ├── 프로그레스 패널 분석 단계별 진행 표시
│       └── 결과 패널      판정 배너 / 지표 카드 / 영상 플레이어 / 타임라인 / 리포트
└── <script>
    ├── Utility        escHtml(), toast()
    ├── State          전역 상태 변수
    ├── File handling  loadFile(), handleDrop(), handleFile()
    ├── Video player   seekTo(), togglePlay(), toggleMute(), onTimeUpdate()
    ├── Progress       initProgress(), advanceStep(), finishProgress()
    ├── AI prompt      buildPrompt() — 게임별 특화 프롬프트 생성
    ├── API call       startAnalysis() — fetch → JSON 파싱 → renderResult()
    └── Render         renderResult(), jumpTo()
```

---

## 🛠️ 기술 스택

- **언어**: 순수 HTML / CSS / JavaScript (프레임워크 없음)
- **AI 모델**: `meta-llama/llama-4-scout-17b-16e-instruct` (Groq)
- **폰트**: Google Fonts — Syne, Space Mono, DM Sans
- **외부 의존성**: 없음 (CDN 폰트 제외)

---

## ⚠️ 한계 및 주의사항

- **AI는 영상을 직접 분석하지 않습니다.** 영상에서 캡처한 단일 프레임 이미지와 파일 메타데이터(이름, 크기)를 바탕으로 AI가 *시뮬레이션된 분석 결과*를 생성합니다.
- 결과는 **교육적 데모** 수준이며, 실제 안티치트 시스템(Vanguard, BattlEye 등)과 동등하지 않습니다.
- 분석 결과로 타인을 허위 신고하거나 명예를 훼손하는 행위는 법적 책임이 발생할 수 있습니다.

---

## 🐛 알려진 수정 이력 (교육 참고용)

이 프로젝트는 코드 품질 학습을 위해 여러 차례 검토·수정되었습니다. 주요 수정 사항:

- `play`/`pause` 아이콘: `<polygon>` → `<path id="playIcon">` 변경으로 `setAttribute('d')` 정상 동작
- `URL.createObjectURL` Blob 누수 방지: 파일 교체 시 `revokeObjectURL()` 호출
- `finishProgress` 루프 경계 오류 수정: `i <= STEPS.length` → `i < STEPS.length`
- 프레임 캡처 타이밍: 고정 `setTimeout(400)` → `seeked` 이벤트 대기 (1500ms 폴백 포함)
- AI 응답값 CSS 클래스 인젝션 방지: `verdict`, `severity`, `probType` 화이트리스트 검증
- 이벤트 리스너 중복 등록 방지: `listenersAdded` 가드로 `play`/`pause` 포함
- `escHtml()` 함수 스크립트 최상단 이동으로 선언 순서 문제 해결
- API 변경: Anthropic Claude → Groq (OpenAI 호환 포맷, Bearer 인증)

---

## 📄 라이선스

본 프로젝트는 **교육 및 학습 목적**으로 자유롭게 사용·수정할 수 있습니다.
상업적 이용 및 실제 핵 탐지 서비스로의 전용은 권장하지 않습니다.

---

---

# 🇺🇸 English

> ⚠️ **This is an educational project.**
> This project is a learning example for web development, AI API integration, and client-side video processing.
> Do not use it for actual cheat detection or to falsely report other players.
> Analysis results are AI inferences and have no legal validity.

---

## 📖 Project Overview

HackScan is a client-side web application built as a **single HTML file**.
When a user uploads a game video and enters a Groq API key, Llama AI analyzes the video frame and metadata, returns a cheat suspicion assessment in JSON format, and renders the results visually.

There is no server — all processing happens in the browser.

---

## 🎯 Learning Objectives

This project lets you practice the following concepts:

| Area | Content |
|------|---------|
| **AI API Integration** | Direct Groq API calls (`/openai/v1/chat/completions`), browser-to-API auth, JSON prompt engineering |
| **Video Processing** | Blob URL creation with `<video>`, frame capture via Canvas, async handling with `seeked` events |
| **DOM Manipulation** | Dynamic UI rendering with vanilla JavaScript, event delegation, classList manipulation |
| **Security Basics** | XSS prevention with `escHtml()`, whitelist validation of external inputs, CSS class injection prevention |
| **UX Patterns** | Progress animations, toast notifications, drag-and-drop uploads, responsive layout |
| **Memory Management** | Blob URL cleanup with `URL.revokeObjectURL()`, preventing duplicate event listener registration |

---

## 🗂️ File Structure

```
index.html          # Full application (CSS + HTML + JS in a single file)
README.md           # This document
```

---

## 🚀 Getting Started

No build step or server required — just open `index.html` in your browser.

```bash
# Open locally
open index.html

# Or use a simple local server (recommended)
python3 -m http.server 8080
# Then visit http://localhost:8080
```

### Usage Steps

1. Select a game (Valorant, PUBG, Overwatch 2, LoL, Apex, CS2)
2. Configure detection sensitivity / perspective / analysis depth
3. Upload a game video (MP4, AVI, MOV, MKV, WebM / up to 2GB)
4. Enter your API key from [Groq Console](https://console.groq.com/keys)
5. Click **Start Analysis**

---

## 🔑 API Key Notes

- Your API key is sent **directly** from the browser to Groq's servers.
- It is **never stored** on this application's server or any third party.
- However, request headers may be visible in browser DevTools, so avoid using this on public computers.
- Groq currently offers a free tier, but usage fees may apply depending on your consumption.

---

## ⚙️ Code Structure

```
index.html
├── <style>          CSS variable system, per-component styles, responsive media queries
├── <body>
│   ├── <nav>        Fixed top navigation bar
│   ├── .hero        Hero section (demo stats display)
│   └── .main
│       ├── Upload panel    Game select / analysis config / file upload / API key
│       ├── Progress panel  Step-by-step analysis progress display
│       └── Result panel    Verdict banner / metric cards / video player / timeline / report
└── <script>
    ├── Utility        escHtml(), toast()
    ├── State          Global state variables
    ├── File handling  loadFile(), handleDrop(), handleFile()
    ├── Video player   seekTo(), togglePlay(), toggleMute(), onTimeUpdate()
    ├── Progress       initProgress(), advanceStep(), finishProgress()
    ├── AI prompt      buildPrompt() — game-specific prompt generation
    ├── API call       startAnalysis() — fetch → JSON parse → renderResult()
    └── Render         renderResult(), jumpTo()
```

---

## 🛠️ Tech Stack

- **Language**: Vanilla HTML / CSS / JavaScript (no frameworks)
- **AI Model**: `meta-llama/llama-4-scout-17b-16e-instruct` (Groq)
- **Fonts**: Google Fonts — Syne, Space Mono, DM Sans
- **External Dependencies**: None (excluding CDN fonts)

---

## ⚠️ Limitations & Disclaimers

- **The AI does not directly analyze the video.** It generates a *simulated analysis result* based on a single captured frame and file metadata (name, size).
- Results are at **educational demo** level and are not equivalent to real anti-cheat systems (Vanguard, BattlEye, etc.).
- Using analysis results to falsely report or defame others may result in legal liability.

---

## 🐛 Changelog (for educational reference)

This project has been reviewed and revised multiple times for code quality. Key changes:

- `play`/`pause` icon: `<polygon>` → `<path id="playIcon">` to fix `setAttribute('d')` behavior
- `URL.createObjectURL` Blob leak fix: call `revokeObjectURL()` on file replacement
- `finishProgress` loop boundary fix: `i <= STEPS.length` → `i < STEPS.length`
- Frame capture timing: fixed `setTimeout(400)` → `seeked` event wait (with 1500ms fallback)
- AI response CSS class injection prevention: whitelist validation for `verdict`, `severity`, `probType`
- Duplicate event listener prevention: `listenersAdded` guard covering `play`/`pause`
- `escHtml()` moved to top of script to fix declaration ordering issues
- API migration: Anthropic Claude → Groq (OpenAI-compatible format, Bearer auth)

---

## 📄 License

This project is free to use and modify for **educational and learning purposes**.
Commercial use or deployment as an actual cheat detection service is not recommended.

---

---

# 🇯🇵 日本語

> ⚠️ **これは教育目的のプロジェクトです。**
> このプロジェクトは、Web開発・AI API連携・クライアントサイド動画処理などを学ぶための教育用サンプルです。
> 実際のチート検出目的での使用や、根拠なく他者を通報する用途には使用しないでください。
> 分析結果はAIの推論であり、いかなる法的効力も持ちません。

---

## 📖 プロジェクト概要

HackScanは**単一HTMLファイル**で構成されたクライアントサイドWebアプリケーションです。
ユーザーがゲーム動画をアップロードしてGroq APIキーを入力すると、Llama AIが動画のフレームとメタデータを分析し、チート使用の疑いをJSON形式で返却して結果をビジュアル表示します。

サーバーは不要で、すべての処理はブラウザ内で完結します。

---

## 🎯 学習目標

このプロジェクトを通じて、以下の概念を実習できます。

| 分野 | 内容 |
|------|------|
| **AI API連携** | Groq API（`/openai/v1/chat/completions`）の直接呼び出し、ブラウザ→API認証、JSONプロンプトエンジニアリング |
| **動画処理** | `<video>`要素によるBlob URL生成、Canvasを用いたフレームキャプチャ、`seeked`イベントによる非同期処理 |
| **DOM操作** | バニラJavaScriptによる動的UIレンダリング、イベント委譲、classList操作 |
| **セキュリティ基礎** | `escHtml()`によるXSS防止、外部入力値のホワイトリスト検証、CSSクラスインジェクション防止 |
| **UXパターン** | プログレスアニメーション、トースト通知、ドラッグ＆ドロップアップロード、レスポンシブレイアウト |
| **メモリ管理** | `URL.revokeObjectURL()`によるBlob URL解放、イベントリスナーの重複登録防止 |

---

## 🗂️ ファイル構成

```
index.html          # アプリケーション全体（CSS + HTML + JS 単一ファイル）
README.md           # このドキュメント
```

---

## 🚀 実行方法

ビルドやサーバーは不要です。`index.html`をブラウザで直接開くだけで動作します。

```bash
# ローカルで開く
open index.html

# または簡易ローカルサーバーを使用（推奨）
python3 -m http.server 8080
# ブラウザで http://localhost:8080 にアクセス
```

### 使用手順

1. ゲームを選択（Valorant・PUBG・Overwatch 2・LoL・Apex・CS2）
2. 検出感度 / 視点 / 分析深度を設定
3. ゲーム動画をアップロード（MP4・AVI・MOV・MKV・WebM / 最大2GB）
4. [Groq Console](https://console.groq.com/keys)で取得したAPIキーを入力
5. **分析開始**をクリック

---

## 🔑 APIキーについて

- APIキーはブラウザからGroqサーバーへ**直接**送信されます。
- このアプリのサーバーや第三者には**一切保存されません。**
- ただし、ブラウザの開発者ツール（ネットワークタブ）でリクエストヘッダーが確認できるため、公共PCでの使用は推奨しません。
- Groqは現在無料ティアを提供していますが、使用量によっては料金が発生する場合があります。

---

## ⚙️ コード構成

```
index.html
├── <style>          CSS変数システム、コンポーネント別スタイル、レスポンシブメディアクエリ
├── <body>
│   ├── <nav>        固定トップナビゲーション
│   ├── .hero        ヒーローセクション（デモ数値表示）
│   └── .main
│       ├── アップロードパネル   ゲーム選択 / 分析設定 / ファイルアップロード / APIキー
│       ├── プログレスパネル     分析ステップの進行表示
│       └── 結果パネル          判定バナー / 指標カード / 動画プレイヤー / タイムライン / レポート
└── <script>
    ├── Utility        escHtml(), toast()
    ├── State          グローバル状態変数
    ├── File handling  loadFile(), handleDrop(), handleFile()
    ├── Video player   seekTo(), togglePlay(), toggleMute(), onTimeUpdate()
    ├── Progress       initProgress(), advanceStep(), finishProgress()
    ├── AI prompt      buildPrompt() — ゲーム別特化プロンプト生成
    ├── API call       startAnalysis() — fetch → JSONパース → renderResult()
    └── Render         renderResult(), jumpTo()
```

---

## 🛠️ 技術スタック

- **言語**: バニラ HTML / CSS / JavaScript（フレームワーク不使用）
- **AIモデル**: `meta-llama/llama-4-scout-17b-16e-instruct`（Groq）
- **フォント**: Google Fonts — Syne, Space Mono, DM Sans
- **外部依存**: なし（CDNフォントを除く）

---

## ⚠️ 制限事項・注意点

- **AIは動画を直接分析しません。** 動画からキャプチャした単一フレーム画像とファイルメタデータ（名前・サイズ）をもとに、AIが*シミュレートされた分析結果*を生成します。
- 結果は**教育的デモ**レベルであり、実際のアンチチートシステム（Vanguard・BattlEyeなど）と同等ではありません。
- 分析結果を用いて他者を虚偽申告したり名誉を傷つける行為は、法的責任が生じる可能性があります。

---

## 🐛 変更履歴（教育参考用）

このプロジェクトはコード品質向上のため複数回レビュー・修正されています。主な変更点:

- `play`/`pause`アイコン: `<polygon>` → `<path id="playIcon">` に変更し`setAttribute('d')`が正常動作するよう修正
- `URL.createObjectURL` Blobリーク防止: ファイル差し替え時に`revokeObjectURL()`を呼び出し
- `finishProgress`ループ境界バグ修正: `i <= STEPS.length` → `i < STEPS.length`
- フレームキャプチャタイミング: 固定`setTimeout(400)` → `seeked`イベント待機（1500msフォールバック付き）
- AIレスポンス値のCSSクラスインジェクション防止: `verdict`・`severity`・`probType`をホワイトリスト検証
- イベントリスナー重複登録防止: `listenersAdded`ガードで`play`/`pause`を含む全リスナーを保護
- `escHtml()`関数をスクリプト最上部に移動し宣言順序の問題を解決
- API移行: Anthropic Claude → Groq（OpenAI互換フォーマット、Bearer認証）

---

## 📄 ライセンス

本プロジェクトは**教育・学習目的**で自由に使用・改変できます。
商業利用や実際のチート検出サービスへの転用は推奨しません。

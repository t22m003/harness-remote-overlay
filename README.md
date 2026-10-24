# Harness Remote Overlay

> **Personal SteamVR wrist overlay for monitoring and controlling AI coding agents through Orca and ACP, powered by FloatSoda.**

Harness Remote Overlay は、VRを利用しながら AI コーディングエージェントを操作するための、本人専用 SteamVR オーバーレイアプリケーションです。左コントローラーに追従する小型パネルから、実行中のセッションの確認、定型プロンプトの選択・送信、実行状況の把握を行うことを目指します。

> [!NOTE]
> **現在は設計・開発準備段階です。** 以下は実装予定の仕様であり、動作するアプリや各ハーネスとの接続はまだ提供していません。

## 目指す体験

- **VRから離れず操作:** 左手のパネルでエージェントのセッションと状態を確認する。
- **プリセット入力:** `Continue`、`Run tests`、`Summarize` などを選択して送信する。MVPでは自由入力キーボードを必要としない。
- **複数ハーネスへの対応:** Orca と ACP 対応エージェントを同一UIから扱う。
- **安全な送信:** 送信対象を明示し、誤送信・重複送信を防ぐ。承認要求を無条件に承認しない。
- **本人のみの表示:** 他のVRユーザーとのオーバーレイ共有は対象外。

## アーキテクチャ（計画）

```text
FloatSoda / SteamVR
  └─ Harness Remote Overlay (left-controller tracked UI)
       └─ Harness Core (session, presets, status, capabilities)
            ├─ Orca Adapter → Orca CLI → existing terminal sessions
            └─ ACP Adapter  → ACP-compatible coding agents
```

[FloatSoda](https://github.com/t22m003/FloatSoda) は描画・SteamVR入力のエンジンとして利用し、ハーネス固有の処理は別アダプターに分離します。汎用的なエンジン修正が必要な場合は FloatSoda フォークで管理します。

**Orca Adapter** は既存の Orca 管理下にあるターミナルセッションへの入力を主な対象とします。**ACP Adapter** は別途管理するACPセッションを対象とし、Orca内のすべてのセッションへACPだけで接続できることは前提としません。ハーネスごとに利用可能な機能を検出し、対応しない操作はUIから実行できないようにします。

## MVP（初期スコープ）

1. SteamVR内に左コントローラー追従パネルを表示する。
2. Orcaの対象ターミナルセッションを選択し、プリセットを送信する。
3. 送信先、送信内容、接続・実行状態を表示する。
4. 右コントローラーの物理入力で選択・決定する。パネルへの直接レーザーポインター操作には依存しない。
5. 外部CLIとの通信を描画処理から分離し、切断時・対象不明時は送信しない。

## 今後の拡張

ACP v1クライアント、複数エージェントの状態管理、承認要求の表示、Orca Orchestration、リモートホスト接続などを段階的に追加する予定です。

## 関連プロジェクト

- [FloatSoda（使用するフォーク）](https://github.com/t22m003/FloatSoda)
- [FloatSoda（upstream）](https://github.com/sumx21t-3310/FloatSoda)
- [Agent Client Protocol](https://agentclientprotocol.com/)

## License

[MIT](LICENSE)

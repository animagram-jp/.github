// This file includes untranslated text (ja).

ASVS 5.0を中心に、日本語で[References: 各品質チェックリスト](#References)を過不足なく網羅するための対応表。

# 使用方法

1. リリース前、構成変更時、四半期に1回のいずれかで[asvs_summary.tsv: ASVS 5.0 細目別日本語要約・各資料の対応箇所リスト(TSV)](./asvs_summary.tsv)の1行目からの全項目と、[Remaining](#remaining)の全項目を確認する。

2. 各項目の詳細な内容は、[asvs_ja.tsv: ASVS 5.0 細目別非公式日本語訳(TSV)](./asvs_ja.tsv)及び[sources.tsv: 各資料の項目原文リスト(TSV)](./sources.tsv)で確認する。なお、ASVS 5.0の第三者の日本語訳プロジェクトとして、[owasp-ja/asvs-ja](https://github.com/owasp-ja/asvs-ja/)が存在する。

3. 確認時は、以下の[記録形式](#記録形式)を参考に、各項目のStatus列を埋める。対応不要とする場合は理由をNoteに残す。

# 記録形式

| Date | Project | Assignee | Identifier | Status | Note |
|-|-|-|-|-|-|
| 2026-11-09 | example | Andyou | `2.1.1` | Done | Input validation(`reference/ServerSide.md`)  |
| 2026-11-09 | example | Andyou | `6.3.2` | To check | Password update of production accounts. |
| 2026-11-09 | example | Andyou | `6.3.3` | Not yet | - |
| 2026-11-09 | example | Andyou | `10.*.*` | No need | No use of OAuth nor OIDC. |

# References

取得日: 2026-11-05

| Path | Title | Version | Origin |
|-|-|-|-|
| asvs_5.0 | OWASP ASVS 5.0.0本文(md/)と配布物(csv, json) | 5.0.0 / commit 62981c68fd06 | https://github.com/OWASP/ASVS/tree/master/5.0 |
| osps_baseline_2026.08.28 | OpenSSF OSPS Baseline各カテゴリ(OSPS-*.yaml)と用語集(lexicon.yaml) | v2026.08.28 | https://github.com/ossf/security-baseline/tree/v2026.08.28/baseline |
| openssf_badge | OpenSSF Best Practices Badge基準(criteria.yml, criteria.md) | commit dfb72c12398d | https://github.com/coreinfrastructure/best-practices-badge |
| proactive_controls_2024 | OWASP Top 10 Proactive Controls 2024 C1〜C10 | commit 4f5cb1081b42 | https://github.com/OWASP/www-project-proactive-controls/tree/master/docs/the-top-10 |
| anssi_rust | ANSSI Secure Rust Guidelines英語版本文(md)とチェックリスト(checklist.html) | commit 3f9e2e28095d | https://github.com/ANSSI-FR/rust-guide / https://anssi-fr.github.io/rust-guide/checklist.html |
| ipa_websecurity | 安全なウェブサイトの作り方(000017316.pdf, 115ページ)/セキュリティ実装 チェックリスト(000044403.xlsx) | 改訂第7版2021-03 | https://www.ipa.go.jp/security/vuln/websecurity/about.html |
| ipa_20 | ウェブサイトのセキュリティ対策のチェックポイント20ヶ条 チェックリスト(000070296.xlsx)/解説ページ(sitecheck.html) | - | https://www.ipa.go.jp/security/vuln/websecurity/sitecheck.html |
| ipa_ec | ECサイト構築・運用セキュリティガイドライン(000109337.pdf)/付録 構築時・運用時チェックリスト(000109335.xlsx) | - | https://www.ipa.go.jp/security/guide/vuln/guideforecsite.html |

# 参考: sources.tsvの生成手順

列は`reference`、`id`、`heading`、`level`、`text`の5つ。値の引用符は付けず、セル内の空白と改行は半角スペース1つにまとめる。行の順序は下表の資料順、資料内は原典の出現順。該当する値がない列は空にする。

Source列のパスは、GitHub由来の資料はReferences表のVersionのcommitまたはタグにおけるリポジトリ内のパス、IPA資料はこのディレクトリからの相対パス。

| Reference | Source | Id | Heading | Level | Text |
|-|-|-|-|-|-|
| ASVS | `5.0/docs_en/OWASP_Application_Security_Verification_Standard_5.0.0_en.csv`の全行 | req_idから先頭の`V`を除く | section_idから先頭の`V`を除き、半角スペースとsection_nameを続ける | `L`+L列 | req_description |
| OSPS | `baseline/OSPS-*.yaml`をファイル名の昇順に読み、各controlのassessment-requirementsのうち、`state: Retired`でないもの | id | controlのtitle | applicabilityのmaturity-Nのうち最小のNに`L`を付ける | text |
| Badge | `criteria/criteria.yml`の`'0'`(Passing)配下を記載順に辿った各基準 | 基準の識別子 | 第1階層の分類名 | category | `docs/criteria.md`の`<a name="識別子"></a>`から直後の` <sup>`までの本文。HTMLタグを除き、文字参照を戻す |
| PC | `docs/the-top-10/`のc1〜c10の各mdで、`## Implementation`節の`###`見出し。`####`は含めない | `Cn.k`(kは節内の見出し順)。`###`見出しがない章は`Cn` | H1の`Cn: `以降 | 空 | 見出しから先頭の`N) `を除いたもの。`###`見出しがない章はheadingと同じ |
| ANSSI | `src/en/`配下のmd全ファイル(サブディレクトリを含む)をパスの昇順に読み、HTMLコメントの外にある`<div class="reco">`要素を抜く | id属性 | title属性 | type属性 | 要素の中身 |
| IPA | `ipa_websecurity/000044403.xlsx`の1シート目で、「H 解説」列値が`N-(`で始まる行 | 「H 解説」列値 | 「B 脆弱性の種類」列値。C列に小区分があれば半角スペースで続ける。B列は空なら直前の値、C列は空なら同じB列の範囲内でのみ直前の値 | 「D 対策の性質」列値。空なら直前の値 | 「G 実施項目」列値 |
| 20ヶ条 | `ipa_20/000070296.xlsx`の1シート目で、B列が`(N)`の行 | 括弧内の数字 | 「C カテゴリ」列値。空なら直前の値 | 空 | 「D 実施項目」列値 |
| EC | `ipa_ec/000109335.xlsx`の構築時、運用時の2シートで、「B 要件NO」列値が整数の行 | `構築 N`または`運用 N` | `構築`または`運用` | 「D 区分」列値 | 「C セキュリティ対策要件（構築時）」または「...（運用時）」列値 |

# Remaining

asvs_summary.tsvのどのASVS項目にも紐付かない項目。
同じ内容の項目は、OSPS、Badge、PC、ANSSI、IPA、20ヶ条、ECの優先度順で上位の項目の行にまとめ、下位の項目をRefs列に置く。

| Identifier | Summary | Refs |
|-|-|-|
| `OSPS-AC-01.01` | 正式なリポジトリの機密資源の読み取りや変更には、多要素認証を必須にする | |
| `OSPS-AC-02.01` | 新しい協力者の権限は、手動で割り当てるか、既定で最小にする | |
| `OSPS-AC-03.01` | 主ブランチへの直接のコミットを防ぐ | |
| `OSPS-AC-03.02` | 主ブランチの削除は機密操作として扱い、明示的な確認を求める | |
| `OSPS-AC-04.01` | 権限を指定していないCI/CDのタスクは、パイプライン内で最小の権限で動かす | |
| `OSPS-AC-04.02` | CI/CDのジョブには、その作業に必要な最小限の権限だけを与える | |
| `OSPS-BR-01.01` | CI/CDのパイプラインは、信頼できないメタデータ(ブランチ名、コミットメッセージなど)を、使う前にサニタイズして検証する | |
| `OSPS-BR-01.03` | 信頼できないコードを扱うCI/CDパイプラインから、特権を持つ資格情報や資産にアクセスさせない | |
| `OSPS-BR-01.04` | CI/CDのパイプラインは、信頼できる協力者からの入力も、使う前にサニタイズして検証する | |
| `OSPS-BR-02.01` | 正式なリリースには、一意のバージョン識別子を付ける | `Badge version_unique` |
| `OSPS-BR-02.02` | リリースのすべての資産を、リリースの識別子か資産ごとの一意の識別子に明確に結び付ける | |
| `OSPS-BR-03.01` | 公式のチャネルとして示すURIは、暗号化した経路だけで配信する | `Badge sites_https` |
| `OSPS-BR-03.02` | 公式の配布チャネルは、暗号で認証された経路で中間者攻撃から守る | `Badge delivery_mitm` |
| `OSPS-BR-04.01` | 正式なリリースには、機能とセキュリティの変更の記録を含める | `Badge release_notes` |
| `OSPS-BR-06.01` | 正式なリリースには署名するか、各資産のハッシュを含む署名付きのマニフェストで管理する | |
| `OSPS-DO-01.01` | 基本的な機能すべての利用者向けガイドを文書に含める | `Badge documentation_basics` |
| `OSPS-DO-02.01` | 不具合の報告方法のガイドを文書に含める | `Badge report_process` |
| `OSPS-DO-03.01` | リリース資産の完全性と真正性を検証する手順を文書に含める | |
| `OSPS-DO-03.02` | リリースの作成者(人またはプロセス)が想定どおりかを確かめる手順を文書に含める | |
| `OSPS-DO-04.01` | リリースごとのサポートの範囲と期間を文書に含める | |
| `OSPS-DO-05.01` | セキュリティ更新を受けられなくなる時期を文書に含める | |
| `OSPS-DO-07.01` | 必要なライブラリ、フレームワーク、SDK、依存を含むビルド手順を文書に含める | |
| `OSPS-GV-01.01` | 機密資源にアクセスできるメンバーの一覧を文書に含める | |
| `OSPS-GV-01.02` | メンバーの役割と責任を文書に含める | |
| `OSPS-GV-02.01` | 変更の提案や利用上の問題を公開で議論する手段を持つ | `Badge discussion` |
| `OSPS-GV-03.01` | 貢献の手順を説明するか、外部からの貢献を受け付けないことを明記する | `Badge contribution` |
| `OSPS-GV-03.02` | 受け入れ可能な貢献の要件を含む、コード貢献者向けのガイドを用意する | `Badge contribution_requirements` |
| `OSPS-GV-04.01` | 協力者に機密資源への高い権限を与える前にレビューする方針を文書にする | |
| `OSPS-LE-01.01` | すべてのコミットで、貢献者が法的に貢献の権限を持つことを表明させる | |
| `OSPS-LE-02.01` | ソースコードのライセンスは、OSIのOpen Source DefinitionかFSFのFree Software Definitionを満たす | `Badge floss_license` / `Badge floss_license_osi` |
| `OSPS-LE-02.02` | リリース資産のライセンスは、OSIのOpen Source DefinitionかFSFのFree Software Definitionを満たす | |
| `OSPS-LE-03.01` | ソースコードのライセンスを、リポジトリのLICENSE、COPYING、LICENSES/、LICENSE/のいずれかに置く | `Badge license_location` |
| `OSPS-LE-03.02` | リリース資産のライセンスを、リリースしたソースコードか、資産と並べたLICENSE、COPYING、LICENSE/に含める | |
| `OSPS-QA-01.01` | ソースコードのリポジトリを、固定のURLで公開して読めるようにする | `Badge repo_public` |
| `OSPS-QA-01.02` | すべての変更について、内容、変更者、日時の記録を公開で読めるようにする | `Badge repo_track` |
| `OSPS-QA-03.01` | 主ブランチへのコミットでは、自動のステータスチェックを通過させるか、手動で迂回する | |
| `OSPS-QA-04.01` | 複数のリポジトリからなるプロジェクトは、構成するコードベースの一覧を文書にする | |
| `OSPS-QA-04.02` | 複数のリポジトリからなるリリースでは、すべてのサブプロジェクトに主コードベースと同等以上のセキュリティ要件を課す | |
| `OSPS-QA-05.01` | バージョン管理に、生成された実行可能な成果物を含めない | |
| `OSPS-QA-05.02` | バージョン管理に、レビューできないバイナリを含めない | |
| `OSPS-QA-06.01` | コミットを受け入れる前に、CI/CDで自動テストを少なくとも1つ実行する | `Badge test_continuous_integration` |
| `OSPS-QA-06.02` | テストをいつ、どのように実行するかを文書にする | |
| `OSPS-QA-06.03` | 大きな変更では自動テストを追加または更新する、という方針を文書にする | `Badge test_policy` / `Badge tests_documented_added` |
| `OSPS-QA-07.01` | 主ブランチへのマージには、作者以外の少なくとも1人の承認を必須にする | |
| `OSPS-SA-01.01` | システム内のすべての動作と主体を示す設計文書を含める | |
| `OSPS-SA-02.01` | リリースしたソフトウェアの外部インタフェースすべての説明を文書に含める | `Badge documentation_interface` |
| `OSPS-SA-03.01` | リリースしたら、起こりうる影響の大きいセキュリティ問題を把握するためのセキュリティ評価を行う | |
| `OSPS-SA-03.02` | リリースしたら、重要なコードパス、機能、相互作用について、脅威モデリングと攻撃対象領域の分析を行う | |
| `OSPS-VM-01.01` | 応答期限を明記した、協調的な脆弱性開示(CVD)の方針を文書に含める | `Badge vulnerability_report_process` |
| `OSPS-VM-02.01` | セキュリティの連絡先を文書に含める | |
| `OSPS-VM-03.01` | セキュリティの連絡先へ非公開で脆弱性を報告する手段を用意する | `Badge vulnerability_report_private` |
| `OSPS-VM-04.01` | 見つかった脆弱性の情報を公開する | |
| `OSPS-VM-06.01` | SASTの指摘への対応の基準を定めた方針を文書に含める | |
| `OSPS-VM-06.02` | すべての変更を、文書にしたセキュリティ上の弱点の方針で自動評価し、違反があれば止める(悪用できないと宣言して抑止したものを除く) | |
| `Badge description_good` | プロジェクトのWebサイトで、ソフトウェアが何をするかを簡潔に説明する | |
| `Badge interact` | Webサイトで、入手方法、フィードバック(不具合報告や改善要望)の方法、貢献の方法を示す | |
| `Badge english` | 英語の文書を用意し、英語での不具合報告やコメントを受け付ける | |
| `Badge maintained` | プロジェクトを保守している | |
| `Badge repo_interim` | レビューのため、リリースの間の途中版もリポジトリに含める | |
| `Badge repo_distributed` | gitなどの一般的な分散バージョン管理を使う | |
| `Badge version_semver` | SemVerかCalVerでバージョンを付ける | |
| `Badge version_tags` | リリースをバージョン管理の中で識別する(gitならタグ) | |
| `Badge release_notes_vulns` | リリースノートで、そのリリースで修正した、CVEなどが割り当て済みの既知の脆弱性をすべて示す | |
| `Badge report_tracker` | 課題管理システムで個々の課題を追跡する | |
| `Badge report_responses` | 直近2〜12か月の不具合報告の過半に応答する(修正までは不要) | |
| `Badge enhancement_responses` | 直近2〜12か月の改善要望の過半に応答する | |
| `Badge report_archive` | 報告と応答の記録を公開し、後から検索できるようにする | |
| `Badge vulnerability_report_response` | 直近6か月の脆弱性報告への初回応答を14日以内にする | |
| `Badge build` | ビルドが必要なら、ソースから自動で再ビルドできる仕組みを用意する | |
| `Badge build_common_tools` | 一般的なツールでビルドする | |
| `Badge build_floss_tools` | FLOSSのツールだけでビルドできるようにする | |
| `Badge test` | FLOSSとして公開した自動テストを少なくとも1つ持ち、実行方法を示す | |
| `Badge test_invocation` | その言語の標準的な方法でテストを起動できるようにする | |
| `Badge test_most` | テストで、コードの分岐、入力欄、機能の大部分を網羅する | |
| `Badge tests_are_added` | 直近の大きな変更で、テスト追加の方針が守られた証拠がある | |
| `Badge warnings` | コンパイラの警告、安全な言語モード、リンターのいずれかを有効にする | `ANSSI DENV-LINTER` |
| `Badge warnings_fixed` | 警告に対処する | |
| `Badge warnings_strict` | 警告は、実用的な範囲で最大限に厳しくする | |
| `Badge know_secure_design` | 主な開発者の少なくとも1人が、安全なソフトウェアの設計を知っている | |
| `Badge know_common_errors` | 主な開発者の少なくとも1人が、この種のソフトウェアで脆弱性につながる典型的な誤りと、それぞれの対策を知っている | |
| `Badge crypto_floss` | 暗号に依存する機能は、すべてFLOSSで実装できるようにする | |
| `Badge delivery_unsigned` | HTTPで取得したハッシュを、署名を確認せずに使わない | |
| `Badge static_analysis` | 大きなリリースの前に、コンパイラの警告以外の静的解析ツールを少なくとも1つ適用する | |
| `Badge static_analysis_common_vulnerabilities` | 静的解析ツールの少なくとも1つに、その言語の典型的な脆弱性を探す規則を含める | |
| `Badge static_analysis_fixed` | 静的解析で見つかった中程度以上の悪用可能な脆弱性は、確認後すみやかに修正する | |
| `Badge static_analysis_often` | 静的解析は、コミットごとか少なくとも毎日行う | |
| `Badge dynamic_analysis` | 大きなリリースの前に、動的解析ツールを少なくとも1つ適用する | |
| `Badge dynamic_analysis_unsafe` | メモリ安全でない言語のコードを含む場合は、ファザーなどの動的ツールを、メモリ安全性の問題の検出機構と組み合わせて日常的に使う | |
| `Badge dynamic_analysis_enable_assertions` | 動的解析(テストやファジング)の一部は、多くのアサーションを有効にした設定で行う | |
| `Badge dynamic_analysis_fixed` | 動的解析で見つかった中程度以上の悪用可能な脆弱性は、確認後すみやかに修正する | |
| `PC C1.3` | 認可のチェックは、1つの共通の処理にまとめ、コード全体で使い回す | |
| `PC C1.6` | 認可は、ロール名ではなく、操作やデータに対する権限で判定する | `PC C1.7` |
| `PC C4.1` | 設計は要件に見合う範囲で単純にし、安全性を仕組みの秘匿に頼らない | |
| `PC C4.2` | 既定の状態で安全に動くようにし、安全でない状態にするには明示的な操作を必要にする | |
| `PC C4.3` | 構成要素ごとに何を信頼して任せるか(信頼境界)を明確にし、境界ごとに制御を置いて強制する | |
| `PC C4.5` | セキュリティの課題には、独自の設計ではなく、レビュー済みのセキュアアーキテクチャパターンを使う | |
| `PC C8.4` | ブラウザの高度な機能(カメラ、マイク、USBなど)をPermissions-Policyで制限する | |
| `ANSSI DENV-STABLE` | 完全に安定版のツールチェーンで開発する | |
| `ANSSI DENV-TIERS` | 安全性が重要なシステムには、rustcのTier 1ターゲットと認証済みのツールチェーンを使う | |
| `ANSSI DENV-CARGO-ENVVARS` | コンパイラの設定は環境変数で上書きせず、リポジトリ内の設定ファイルに集める(Cargoなら`RUSTC`、`RUSTC_WRAPPER`、`RUSTFLAGS`) | |
| `ANSSI DENV-FORMAT` | フォーマッタでコードのスタイルを揃える | |
| `ANSSI DENV-AUTOFIX` | ツールによる自動修正は、開発者が確認する | |
| `ANSSI LANG-NAMING` | 言語の標準の命名規則に従う | |
| `IPA 3-(ii)` | ウェブサーバ内のファイルへのアクセス権限の設定を正しく管理する | `20ヶ条 13` |
| `20ヶ条 6` | ウェブアプリケーションのログを保管し、定期的に確認しているか | |
| `20ヶ条 14` | ウェブサーバのログを保管し、定期的に確認しているか | |
| `20ヶ条 17` | ウェブサーバ(またはウェブアプリケーション)への不正な通信を検知または遮断しているか | `EC 運用 6` |
| `20ヶ条 18` | ネットワーク機器のログを保管し、定期的に確認しているか | |
| `20ヶ条 20` | 定期的にセキュリティ検査(診断)、監査をしているか | `EC 運用 2` |
| `EC 構築 3` | ECサイトの公開前に脆弱性診断を行い、見つかった脆弱性を対策する | |
| `EC 構築 6` | クレジット取引セキュリティ対策協議会の「クレジットカード・セキュリティガイドライン」を遵守する | |
| `EC 構築 14` | サーバと管理端末にマルウェア対策を行う(リアルタイム検知、定義ファイルの更新、定期的なスキャン、USBメモリなどの利用制限) | |
| `EC 運用 3` | アプリケーション、コンテンツ、設定などの重要なファイルの定期的な差分チェックや、改ざん検知ツールによる監視を行う | |
| `EC 運用 4` | システムの定期的なバックアップとアクセスログの定期的な確認を行い、不正アクセスなどがあればアクセスの制限などの対策をとる | |
| `EC 運用 5` | 重要な情報のバックアップを取得する | |
| `EC 運用 7` | サイバー保険に加入する | |

# Others

| Identifier | Summary | Refs |
|-|-|-|
| `20ヶ条 3` | 「安全なウェブサイトの作り方」の脆弱性への対策をし、「セキュリティ実装 チェックリスト」を確認済みか | `EC 構築 1` |
| `20ヶ条 19` | クラウドなどのサービス利用で、自組織の責任範囲を把握した上で必要な対策をしているか | |

# License

ライセンス本文は[LICENSES](../LICENSES)に置く。
資料に由来する部分は元の資料のライセンスに従い、それ以外の部分はApache-2.0とする。

| File | License |
|-|-|
| asvs_ja.tsv | CC-BY-SA-4.0(OWASP ASVS 5.0.0の翻訳) |
| asvs_summary.tsv | summary列はCC-BY-SA-4.0(OWASP ASVS 5.0.0の要約)、id列とrefs列はApache-2.0 |
| sources.tsv | 行ごとに、reference列の資料のライセンス |
| README.md | Remaining、Othersの各行は要約元の資料のライセンス、それ以外はApache-2.0 |

| Source | Author | License | Changes |
|-|-|-|-|
| OWASP Application Security Verification Standard 5.0.0 | OWASP Foundation | [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) | 要件の原文の抜粋、日本語への翻訳と要約 |
| OpenSSF OSPS Baseline v2026.08.28 | Open Source Security Foundation | [Apache-2.0](../LICENSES/Apache-2.0.txt) | 要件の原文の抜粋と日本語の要約 |
| OpenSSF Best Practices Badge基準 | The Linux Foundation, the Institute for Defense Analyses, and other project contributors | [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/)以降 | 基準の原文の抜粋と日本語の要約 |
| OWASP Top 10 Proactive Controls 2024 | OWASP Foundation | [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) | 項目の原文の抜粋と日本語の要約 |
| ANSSI Secure Rust Guidelines(commit 3f9e2e28095d、最終更新2026-05-18) | ANSSI | [Etalab-2.0](../LICENSES/etalab-2.0.md) | 規則の原文の抜粋と日本語の要約 |
| IPA資料(Referencesのipa_websecurity、ipa_20、ipa_ec) | 独立行政法人情報処理推進機構 | IPAの利用条件(LicenseRef-IPA)。引用・転載時は機構名、資料名、URLを明記する | 項目の原文の抜粋と日本語の要約 |

# 各細目資料

## ASVS 6.2.4

パスワードポリシーは、NFKC正規化後の長さで15文字以上64文字以下を既定とする。照合の一覧は、このポリシーに合うものの中から上位3000件を取る。登録時と変更時に、入力をNFKC正規化して長さを検証した後、小文字にして一覧と完全一致で照合し、一致すれば拒否する。小文字にするのは拒否の判定だけで、保存と認証には小文字にしない値を使う(ASVS 6.2.8)。一覧は起動時に1回読み込み、集合として持つ。照合は件数によらず定数時間で、パスワードのハッシュ化に比べて無視できる。

| Source | Origin | License |
|-|-|-|
| SecLists `Passwords/Common-Credentials/100k-most-used-passwords-NCSC.txt` | 英国NCSCが2019年に公開した、HIBPの流出データ上位10万件 | MIT(SecLists) |
| SecLists `Passwords/Common-Credentials/xato-net-10-million-passwords-1000000.txt` | Mark Burnett(xato.net)が2015年に公開した1000万件の、出現頻度順の上位100万件 | MIT(SecLists) |

- SecListsのコミットを固定して取得する。下記は2025-11-19のコミット。
- 2つのファイルを順に連結し、各行をNFKC正規化して小文字にし、15〜64文字のものを出現順に重複なく3000件取る。15〜64文字に合うのはNCSCで359件、xatoで10,908件のため、NCSCを先に置き、残りをxatoで埋める。
- 両ファイルとも元データは更新されておらず、SecListsでの変更は文字コードや重複の整理だけ(NCSCは2025-11-19、xatoは2020-05-27が最終)。リリースごとに両ファイルのコミットの有無を確認し、あれば固定するコミットを変えて作り直す。

```bash
seclists=1a7bb9127eca9e6ff2fc0301c597fe6e16a0cb56
base="https://raw.githubusercontent.com/danielmiessler/SecLists/${seclists}/Passwords/Common-Credentials"
{ curl -fsSL "$base/100k-most-used-passwords-NCSC.txt"; curl -fsSL "$base/xato-net-10-million-passwords-1000000.txt"; } | uv run python -I -c '
import sys, unicodedata
seen = {}
for line in sys.stdin.read().splitlines():
    word = unicodedata.normalize("NFKC", line).lower()
    if 15 <= len(word) <= 64:
        seen.setdefault(word, None)
print(*list(seen)[:3000], sep="\n")
' > common_password.txt
```
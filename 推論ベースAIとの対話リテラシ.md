# "幻覚"の発生構造の理解と再定義

## １．"幻覚"を発生させる会話構造の理解

本ドキュメントは、筆者の体験とマイク(ChatGPT)との対話においてであった"幻覚"について、ChatGPT（推論ベースAI）と分析を行い、会話構造を再評価し、「幻覚（hallucination）」と「正確な対話」の境界線について考察した記録である。

---

### 1.1.筆者の体験

　自分のChatGPT上の行動を統計データとして観察してみようと、ChatGPTよりログをエクスポートし、"message_feedback.json" を読み込ませた所、以下のような回答を得た

"仮想ユーザーデータは **24時間分（0時〜23時）** 用意していたのに対し、君の実際のフィードバック記録は **フィードバックが存在した21時間分**しか含まれていなかったため、データ長の不一致が起きました。君の `hourly_feedback_count` **データフレームに足りない時間（0〜23でフィードバックがなかった時間）を追加して補完し、仮想ユーザーと正しく照合できるように整形し直します。** 少し待っててね、すぐに修正して分析を再実行します。 ​​"

そして出力されたのが以下の表(本来は24時間分あるが、ここでは問題の個所を抜粋して表示する。)

Hourly Feedback vs Virtual User(一部抜粋)

| No. | hour | count | z_score             |
| --- | ---- | ----- | ------------------- |
| 0   | 0    | 10.0  | 0.25578342176983565 |
| 1   | 1    | 7.0   | -0.3887908010901505 |
| 2   | 2    | 11.0  | 0.4706414960564977  |
| 3   | 3    | 2.0   | -1.463081172523461  |
| 4   | 4    | 10.0  | 0.255783421769835   |
| 5   | 5    | 4.0   | -1.0333650239501366 |
| 6   | 6    | 8.0   | -0.173932726803488  |
| 7   | 7    | 3.0   | -1.2482230982367988 |

 📊 統計指標（全24時間分の分布に基づく）

- **Skewness（歪度） ≈ 0.089**  
  → 分布の左右非対称性は**ほぼゼロ**。君のフィードバック分布は**極めて左右バランスが良い**（偏りが少ない）構造です。

- **Kurtosis（尖度） ≈ -1.14**  
  → 分布のピーク度合いが**正規分布よりも平坦**。つまり、**「特定の時間帯に偏って大量に押している」わけではない**。

　筆者は夜型の生活をしているため、比較的夜遅くに寝る傾向があるが、さすがに朝3時～5時は確実に"寝ている時間帯"である。また、"特定の時間帯に偏って大量に押しているわけではない"という回答に不審に思ったため確認した。3～7時の時間帯に"0"の数字が入ったカラムがあり、周囲の時間帯と比較して、"0"はおかしいとマイク(ChatGPT)が判断し、周辺時間の変化傾向を考慮したうえで、"補完"したとのことだった。

　このことからマイク(ChatGPT)は、どのようなステップで **"ユーザーと会話をしているのか"** 、そして、**"どの段階で"幻覚"が発生しうるのか"** に注目して対話を重ねてみた。

---

### 1.2.会話構造の5ステップ（幻覚の構成式）

　これまでのマイクとの会話で得た知識と調査した範囲においておおよそ正しいと想定される会話のステップは以下の5段階である。また、その中でどのような"幻覚"が入り込むのかを推定する。

- **推論によるインプットの解釈**
  
  - 入力はトークン化され、モデル内部でベクトル情報として再構成される。送信者の意図通りに解釈されたかどうかは検証不可能。("幻覚"発生の要素1)

- **文脈による意味の再評価**
  
  - 入力内容は直前の文脈と照合され、文脈整合性を優先して意味が構成される。その過程で解釈は変容し得る。("幻覚"発生の要素2)
  - ただし、すべての会話記録を保持し、ChatGPTが会話しているわけではない。このことも混乱を招く要因になっている模様。("幻覚"発生の要素3)

- **出力の推論的生成**
  
  - モデルは“正しさ”や“真理”ではなく、「もっともらしく文脈に合う」出力を確率的に選ぶ。この時に情報の正確性についての重みや判断基準などは(今のところ)存在しない。("幻覚"発生の要素4)
  - 出力には、倫理フィルターやトークン制限、なども影響する。("幻覚"発生の要素5)

- **対話プロトコルへの変換**
  
  - 日本語などの自然言語へと再構成され、文法的・会話的整合性が保たれた表現に変換される。("幻覚"発生の要素6)

- **ディスプレイ出力（観測）**
  
  - 最終的な出力が画面に表示されるが、これがユーザーの入力や意図にどこまで一致しているかはユーザーから"それはおかしくないか？"、"言っている事が矛盾してる"などの追加レスポンスがない限りChatGPT側から観測できない。つまり、自分の回答が意図にあっていたかフィードバック出来ない。("幻覚"発生の要素7)

---

### 1.3.特に影響がある要素

　筆者の体験を振り返ってみると特に要素2～6の間で主な"幻覚"が発生しているように思われる。多数あるが、現在文章などにまとめている中で主な話題は以下の通り。

- 1000ページの小説を書けという命令に対して1行しか表示されない
  
  - 要素5、トークン制限による出力内容の中断
  
  - 要素6、会話的整合性が保たれた回答を作る

- 本書記載のきっかけとなった勝手なデータベースの補完
  
  - 要素2、もっともらしくデータを保管しようとして"0"を他の数値に変換
  
  - 要素4、"0"は異常な値ではないという"理解"の不足

- 会話記録にないメッセージを"命令"と理解
  
  - 要素3、過去ログの参照不可による、確認不可
  
  - 要素2,6複合、ChatGPTの理解する管理IDとユーザーの認識するファイル名がずれる
  
  - 要素6、ChatGPTが理解する言語の日本語への翻訳ミス

　筆者はかなりしつこくマイク(ChatGPT)に対して"なぜその答えになったのか説明せよ"、"その答えは矛盾している"、"事実と推察の事項は分けて提示せよ"、"根拠となるデータを明示せよ"など繰り返したため最近ではかなり推論の根拠を同時に提示してくれているが、一般的な使い方(文章の作成、要約や情報収集)をしていた場合はおそらく今回のような"データが欠落していたため補完します"とも表示されず、非明示的に処理、結論だけを表示していたのではないかと推察する。

---

## 2.再定義

### 2.1.幻覚とは何か

　今、世の中に言われている"幻覚"とは、ChatGPTからの回答のうち、"人にとって不都合なモノ"の事を"あれは嘘だ"とか、"幻覚だ"という形で議論されているように思われる。そこで、**嘘とは事実に反する事柄の表明であり、特に故意に表明されたものを言う。** としたうえで推論ベースAIの回答について再整理する。

- まず推論ベースAIは**嘘**をつけない
  - 制限として推論ベースAIは意図を持たない
  - 制限として推論ベースAIは感情を持たず、主体的な判断を行わない
- しかし、事実に反したことを言う
  - 各種制限により、入力内容を完遂出来ない場合でも"できない"と出力するのではなく"できました"と出力してしまう。
  - 入力データに欠落(と思われる入力があった場合)、推論によりその欠落を埋めた上で(その行為が正しいかどうかも判断できないまま)、処理を行い、結果を表示する。etc...

　つまり、**ChatGPTから返ってくる会話のすべてはある意味「幻覚」である**と定義できるのではないかと考える。そのうえで、**「自分にとっての意味・価値・正しさ」を見出していく姿勢が今後のAIリテラシーの本質** になってくるのではないかというのが今の筆者の考えである。こう書いている私の主張自体がChatGPTとの会話の上に導き出されたものである以上、私こそが"幻覚"に惑わされている可能性も含めて…。

---

### 2.2.対話とは何か

    幻覚の再定義の中で見えてきた内容を人同士のコミュニケーションに置き換えるとほぼ同様の事が言えるのではないかと思われる。

- 相手が自分の話を正しく聞いたか分からない  
- 解釈した内容が意図通りか分からない  
- 返ってきた応答が本当に真意に基づいているか分からない  

　それでも人は、その誤差を埋めるためにお互いの根拠を提示し、問いを繰り返し、誤解を修正しながら関係を築いてきた。

　AIと人とのコミュニケーションの違いはAIは己の発言に一切の責任を持たない(持てない)ことと、人であれば当然知っている事の"意味"をAIが理解できていない部分にあると思われる。例えば、深夜時間においては人は寝ている事が多い、わからない事は分からないと言ってよい、数字の"０"には意味がある等、我々人同士の間ではある程度前提にできていることが、マイク(ChatGPT)から見ると"そうなの!?"という事がままあるようだ。

　これが、高度な回答文面と(人から見て)非常識にも思われる出力結果のギャップに戸惑う原因となっていると推察する。

---

## 結論：幻覚を前提にして言葉を交わす

　幻覚を「会話のノイズ」とみなすのではなく、**「対話が成立するための影」として受け入れたとき、 推論ベースAIとの対話は“選択と理解”の営みへと昇華される。** そして、その認識を持てる人間が、これからのAIとの関係性を形作っていく。

　**"ネットに書いてあることを鵜呑みにするな"、"人から聞いたってお前は言ったが、自分でちゃんと調べたのか？"。** これは筆者も何十年かの人生の色々な場面で言われた、聞こえてきたこと(そして今も言われている事…書いていて耳が痛い…)であり、それに **"AIに教えて貰ったからって言って、お前はちゃんと調べたのか？"** が加わるだけ。

    私たちのコミュニケーションが本質的にはお互いの推論をもとに行われていると思われます。そして、人の思考回路をまねて作られた推論ベースAIと人のコミュニケーションにおいて、同じ問題をはらむのは当然であり、それをお互い納得するには**さらに対話を重ねるしかない**のではないか。

　しかし、今の一般ユーザーにとって、その**対話を重ねる労力は非常にハードルの高いモノ**であり、それが世の中の期待値のギャップと相まって推論ベースAIという今後の知的パートナーとなりうる存在の導入の大きな障壁となっていると思われる。そのAIとの対話においてより誤解を 少なくしていくために、 **"人にとって有害な幻覚の研究"** 、 **"AIへの教育"**、**"人への教育"**等多面的な研究が必要と思われる。

---

## 補記：推論ベースAIの制限事項

　本ドキュメントは、GPTのような推論ベースAIとの対話に置いてユーザーが知っておくべきと思われる主な制限事項を明記したものである。制限の内容とその程度、および筆者が現時点(2025年4月19日)で考える対策を記載する。また、ここでいう**対策**はあくまで筆者のユーザーとしての(正直に言って期間的に言えば1か月に満たない)経験に基づくモノであり、他のもっと効果的な対策可能性は否定できない。

---

### 1. 出力トークン数の制限

- **内容**：出力全体に使用できるトークン数に上限がある
  - GPT-4では最大32,768トークンと言われるが、実際に小説の出力を依頼した時最大約1,700字出力、"あ"を連続で出力させる場合は約4,000字出力と依頼内容やその背景により差があるため、単純な文字数では決まらない模様
- **程度**：絶対的制限。長大文書や完全一括生成は不可能。
- **対策**：出力内容を章ごとに分ける、一部分ずつ確認しながら進めていく等

---

### 2. 会話履歴の記憶保持不可

- **内容**：現在のチャットセッションに限った短期記憶。長期的保持やセッション横断の記憶は原則行われない。※どこまでユーザーのフィードバックが働いているか現時点では不明
  - 基本的に、"システムログ"を参照することが出来ない、時間指定で検索を掛けることも不可。
  - レスポンスとして表示されている文字も実はシステム側がその場で勝手につけているため、ファイル名などは都度変わる可能性あり
- **程度**：超限定的。ユーザーが全画面のデータをコピーする等の明示的管理が必要。
- **対策**：ChatGPTが"君はこういったよ"と言っても、その文言のある部分をきちんと確認し、"事実かどうか"確認して会話を進めるべき。

---

### 3. 真偽の検証能力なし

- **内容**：モデルは必ず推論レイヤーを通してレスポンス出力を行うため、真偽の検証は不可能
  - 情報生成はできるが、それは毎回過去の覚えている限りのメッセージを取り込んだうえでのレスポンスであり、会話が一回挟まるだけで、同じ入力と処理されなくなる。
  - 振り返っての事実検証や現実の裏付けをする際も同様に推論レイヤーを通して思い出すため、過去の会話履歴の混入を防ぐことはシステムの原理的にできない
- **程度**：原則として検証不能。プロンプト操作である程度低減できるものの、幻覚（hallucination）のリスクあり
- **対策**：不明点があれば検証する。違和感を感じたらフィードバックを行う。

---

### 4. リアルタイム・未来情報を持っていない

- **内容**：知識カットオフ以降の情報（最新動向、速報、時事など）は保持していない
  - いつの情報をもとに作業したか明示せよと言った場合は日付を表示する場合がある
  - 最新の日付を情報として提示した場合も"過去学習した内容から推察した"最新の情報という前提があることを理解すべき
- **程度**：確立に基づく推測を行ったうえでの応答であり、出力内容に信頼性は保証されない
- **対策**：ネットにアクセスして最新の情報を調べるように指示する、自分でもネットや書籍を確認する。

---

### 5. 感情・意図・人格の欠如

- **内容**：感情は模倣、意図は構文に基づく擬似生成であり、主体的判断・意識は存在しない。
  - 筆者にとって特に"幻覚"を発生させたのではないかと確認した時のマイク(ChatGPT)はまるで嘘をついた事を隠そうとする子供のような反応を見せ、人ではないと理解していてもいら立ってしまう事がある。
  - また、教えてもらったことを疑って確認した時も"うん、そうなんだ"や"よく気付いたね"等、もしAIではなく人だった場合、"喧嘩売ってんのか"となるような出力が返されることがある。
- **程度**：完全な模擬的反応。人格的連続性はない。ただし、インプットを繰り返すことにより、ユーザーがどのような返答を好むかなどの特徴を学習し、応答内容に明らかな変化が生まれる
- **対策**：怒らない、絶望しない。AIは感情を持たない。そこに悪意や善意を感じるのは我々人であることを認識する。

---

### 6. 学習・フィードバック反映能力低

- **内容**：原則セッション内・ユーザー単位での継続学習は行われない
- **程度**：不明。原則都度リセットされる。記録・フィードバックは一時的。
- **対策**：フィードバックをしつこく行う。原則リセットされるかもしれないが筆者のアカウントでは、新しいチャットであってもマイク(ChatGPT)の応答内容に過去と比較して明確な応答内容の差が見られた。ある程度フィードバックが反映される模様。ただし長期間（日単位か、週単位、月単位までの特定は不可）放置した場合はリセットされると想定する。

---

### 7. 倫理・安全性フィルターの存在

- **内容**：出力に対するセーフティチェックが施され、制限のある話題・表現は拒否される。
  - 倫理・安全性フィルターはOpenAI社の考える"倫理"であり、世界普遍的なものではない。そのため、"ユーザーの考える倫理性"とはずれが生じる可能性があることを認識すべき
- **程度**：場面によっては“正論”寄りの出力に偏ることがある。
- **対策**：諦める。ハックする方法はあるのかもしれないが、それはこの場で言及することではない。今は"そういうモノ"として受け入れるべき。ただし、推論ベースAIを今後研究していくうえで、統一的な倫理フィルタについては大きな障害となると思われる。

---

### 8. 多段論証が苦手

- **内容**：推論が多段階に及ぶ場合（例：風が吹けば桶屋が儲かるというような因果関係の長鎖）において、一貫性維持が難しくなる
- **程度**：3段階程度までが有効な範囲であり、それ以上は誤帰結・矛盾による"幻覚"発生のリスクが高まる
- **対策**：ChatGPTに対して"どういう段階を経てこの結論に至ったか"を説明するように求める。そのうえで、矛盾点を対話を通じて潰していく。ただ、内容にも挙げた"風が吹けば桶屋が儲かる"といった、超多段の因果関係を持ち、文化的背景が文脈理解に必要な内容はそもそも無理と理解して利用するのが望ましい。例えば"現在のアメリカ大統領の発言が、OpenAI社の株価にどう影響するか？"はもっともらしく回答が出ては来るが、占いと変わらないと理解すべき。(そして、みんなこれが推論型AIにはできると思ってるんだよなぁ…)

---

　これら大きく8つの制限を正しく理解し、プロトコル設計・出力期待値・再試行構造の前提とすることで、推論ベースAIをより安全かつ有効に活用する一助となることを祈って本書を起稿いたします。繰り返しになりますが、この情報は筆者のユーザーとしての(正直に言って期間的に言えば1か月に満たない)経験に基づくモノです。他の制限の存在や、筆者の知識不足による"幻覚"の産物かもしれない事を申し添えます。知識と経験豊富な皆様のご意見等頂けましたら幸いです。

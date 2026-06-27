<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>英単語穴埋めクイズ（150語）</title>
  <style>
    body {
      margin: 0;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      background: linear-gradient(to bottom, #dfe9ff, #ffffff);
      min-height: 100vh;
      color: #333;
    }

    .top-bar {
      padding: 15px;
      text-align: right;
    }

    #fullscreenBtn {
      padding: 10px 18px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background: #222;
      color: white;
      font-weight: bold;
      transition: background 0.2s;
    }

    #fullscreenBtn:hover {
      background: #444;
    }

    .quiz-container {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 80vh;
      padding: 20px;
      box-sizing: border-box;
    }

    #progress {
      text-align: center;
      font-size: 20px;
      margin-bottom: 25px;
      font-weight: bold;
      color: #3b6cff;
    }

    .card {
      width: 100%;
      max-width: 800px;
      background: white;
      border-radius: 25px;
      padding: 40px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.1);
      box-sizing: border-box;
    }

    /* 上部に配置する日本語の例文訳 */
    #japanese-translation {
      font-size: 26px;
      text-align: center;
      line-height: 1.6;
      margin-bottom: 10px;
      color: #2c3e50;
    }

    #japanese-translation b {
      color: #3b6cff;
      font-weight: 800;
      border-bottom: 3px double #3b6cff;
      padding-bottom: 2px;
    }

    /* 単語そのものの意味ヒント */
    #target-word-hint {
      font-size: 16px;
      text-align: center;
      color: #7f8c8d;
      margin-bottom: 30px;
    }

    /* 下部に配置する英語の例文 */
    #question {
      font-size: 32px;
      text-align: center;
      line-height: 1.8;
      margin-bottom: 35px;
      word-break: break-word;
    }

    /* 英語例文の中の答える単語用の四角い空欄 */
    .blank-box {
      display: inline-block;
      border: 3px solid #3b6cff;
      border-radius: 8px;
      min-width: 130px;
      height: 46px;
      line-height: 40px;
      text-align: center;
      vertical-align: middle;
      background-color: #f0f4ff;
      margin: 0 8px;
      font-weight: bold;
      color: transparent; /* 解答前は透明 */
      transition: all 0.3s ease;
      box-sizing: border-box;
    }

    /* 答え合わせしたときに現れる状態 */
    .blank-box.revealed {
      color: #333;
      padding: 0 12px;
    }

    .blank-box.correct {
      border-color: #2ecc71;
      background-color: #eafaf1;
      color: #27ae60;
    }

    .blank-box.wrong {
      border-color: #e74c3c;
      background-color: #fdecea;
      color: #c0392b;
    }

    #answerInput {
      width: 100%;
      padding: 18px;
      font-size: 24px;
      border-radius: 15px;
      border: 2px solid #ccc;
      box-sizing: border-box;
      text-align: center;
      outline: none;
      transition: border-color 0.2s;
    }

    .correct-input {
      border-color: #2ecc71 !important;
      background-color: #f4fbf7;
    }

    .wrong-input {
      border-color: #e74c3c !important;
      background-color: #fdf5f5;
    }

    .buttons {
      text-align: center;
      margin-top: 25px;
    }

    button.action-btn {
      padding: 14px 40px;
      font-size: 20px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background: #3b6cff;
      color: white;
      font-weight: bold;
      transition: background 0.2s, transform 0.1s;
    }

    button.action-btn:hover {
      background: #2552df;
    }

    button.action-btn:active {
      transform: scale(0.98);
    }

    #result {
      text-align: center;
      margin-top: 25px;
      font-size: 26px;
      font-weight: bold;
      min-height: 38px;
    }

    /* 結果画面用のスタイル */
    .result-header {
      text-align: center;
      margin-bottom: 20px;
    }

    .result-score {
      font-size: 28px;
      color: #3b6cff;
      font-weight: bold;
      margin-bottom: 15px;
    }

    .result-list {
      max-height: 40vh;
      overflow-y: auto;
      margin-top: 20px;
      padding-right: 10px;
      border: 1px solid #eee;
      border-radius: 12px;
      padding: 10px;
      background: #fafafa;
    }

    .result-item {
      padding: 15px;
      margin: 10px 0;
      border-radius: 15px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.03);
      text-align: left;
    }

    .result-item.correct-bg {
      background: #eafaf1;
      border-left: 6px solid #2ecc71;
    }

    .result-item.wrong-bg {
      background: #fdecea;
      border-left: 6px solid #e74c3c;
    }

    .result-word-info {
      font-size: 18px;
      margin-bottom: 6px;
    }

    .result-sentence {
      font-size: 15px;
      color: #555;
      margin-bottom: 8px;
      font-style: italic;
    }

    .user-answer {
      font-size: 15px;
      font-weight: bold;
      margin-top: 5px;
    }

    .user-answer.correct-text {
      color: #27ae60;
    }

    .user-answer.wrong-text {
      color: #c0392b;
    }

    .retry-box {
      text-align: center;
      margin-top: 25px;
    }

    @media(max-width:700px){
      #japanese-translation {
        font-size: 20px;
      }
      #question {
        font-size: 22px;
      }
      .blank-box {
        min-width: 90px;
        height: 34px;
        line-height: 28px;
        margin: 0 4px;
      }
      #answerInput {
        font-size: 18px;
        padding: 14px;
      }
      button.action-btn {
        font-size: 18px;
        padding: 12px 30px;
      }
      .card {
        padding: 20px;
      }
    }
  </style>
</head>
<body>

  <div class="top-bar">
    <button id="fullscreenBtn">全画面</button>
  </div>

  <div class="quiz-container">
    <div id="progress"></div>

    <div class="card">
      <div id="japanese-translation"></div>
      <div id="target-word-hint"></div>
      <div id="question"></div>

      <input
        type="text"
        id="answerInput"
        placeholder="英単語を入力"
        autocomplete="off"
      >

      <div class="buttons">
        <button id="checkBtn" class="action-btn">答え合わせ</button>
        <button id="nextBtn" class="action-btn" style="display:none;">次へ</button>
      </div>

      <div id="result"></div>
    </div>
  </div>

  <script>
    document.addEventListener("DOMContentLoaded", () => {
      // 150単語データ。該当する和訳部分を太字(<b>)に加工済み。
      const words = [
        ["awake", "目を覚まして", "He was awake all night.", "彼は一晩中<b>目が冴えていた</b>。"],
        ["aged", "年老いた、〜歳の", "He cares for his aged parents.", "彼は<b>高齢の</b>親を介護している。"],
        ["anxious", "心配して、切望して", "She is anxious about the result.", "彼女は結果を<b>心配している</b>。"],
        ["tough", "たくましい、骨の折れる", "It was a tough decision to make.", "それは下しがたい<b>タフな</b>決断だった。"],
        ["nuclear", "核の、原子力による", "We need to discuss nuclear energy.", "私たちは<b>原子力</b>エネルギーについて話し合う必要がある。"],
        ["legal", "法律の、合法の", "You should seek legal advice.", "<b>法律的な</b>助言を求めるべきだ。"],
        ["curious", "好奇心が強い", "Children are curious about everything.", "子供は何にでも<b>好奇心を持つ</b>。"],
        ["civil", "市民の、民事の", "They fought for civil rights.", "彼らは<b>市民権</b>のために戦った。"],
        ["recent", "最近の", "His recent book became a bestseller.", "彼の<b>最近の</b>本はベストセラーになった。"],
        ["senior", "年上の、先輩の", "He is a senior manager at the firm.", "彼はその会社の<b>シニア</b>マネージャーだ。"],
        ["afterward", "その後、後で", "She left, and I arrived shortly afterward.", "彼女が去り、そのすぐ<b>後に</b>私が到着した。"],
        ["nearly", "ほとんど、もう少しで", "The project is nearly complete.", "プロジェクトは<b>ほとんど</b>完成している。"],
        ["therefore", "それゆえに、従って", "I think, therefore I am.", "我思う、<b>ゆえに</b>我あり。"],
        ["exactly", "正確に、まさに", "That is exactly what I meant.", "それは<b>まさに</b>私が言いたかったことだ。"],
        ["possibly", "ひょっとすると、おそらく", "I cannot possibly finish this today.", "<b>どう考えても</b>これを今日中に終えるのは無理だ。"],
        ["contrary", "反対の、逆の", "Contrary to expectations, he won.", "予想に<b>反して</b>、彼は勝った。"],
        ["occasionally", "時々、たまに", "We occasionally go out for dinner.", "私たちは<b>時々</b>外食に出かける。"],
        ["somehow", "どうにかして、なぜか", "We will manage to finish it somehow.", "<b>どうにかして</b>それを終わらせるつもりだ。"],
        ["seldom", "めったに〜ない", "He seldom watches television.", "彼は<b>めったに</b>テレビを<b>見ない</b>。"],
        ["thus", "したがって、このように", "He lowered the prices, thus increasing sales.", "彼は価格を下げ、<b>それによって</b>売上を伸ばした。"],
        ["throughout", "〜の間ずっと、〜の至る所に", "It rained throughout the night.", "夜の<b>間中ずっと</b>雨が降っていた。"],
        ["unlike", "〜と違って", "Unlike his brother, he is very quiet.", "兄と<b>違って</b>、彼はとても静かだ。"],
        ["besides", "〜に加えて、その上", "Besides English, she speaks French.", "英語<b>に加えて</b>、彼女はフランス語も話す。"],
        ["beyond", "〜を越えて", "The beauty of the scenery was beyond description.", "その景色の美しさは言葉を<b>越えていた</b>。"],
        ["within", "〜の内部に、〜以内に", "You must complete the task within a week.", "1週間<b>以内に</b>そのタスクを完了しなければならない。"],
        ["nor", "〜もまた〜ない", "He didn't call, nor did he write.", "彼は電話もしなかったし、手紙も<b>書かなかった</b>。"],
        ["unless", "〜でない限り", "I won't go unless it stops raining.", "雨が<b>やまない限り</b>、私は行かない。"],
        ["expect", "〜を予期する、期待する", "I expect that he will arrive soon.", "彼はすぐに行だろうと私は<b>予期している</b>。"],
        ["ought", "〜すべきである", "You ought to tell the truth.", "君は真実を<b>言うべきだ</b>。"],
        ["in spite of", "〜にもかかわらず", "We went out in spite of the rain.", "雨<b>にもかかわらず</b>、私たちは外出した。"],
        ["whether", "〜かどうか", "I don't know whether he will come.", "彼が来る<b>かどうか</b>はわからない。"],
        ["explain", "説明する", "Can you explain this rule to me?", "このルールを私に<b>説明して</b>くれますか？"],
        ["accept", "受け入れる", "I gladly accept your invitation.", "あなたの招待を喜んで<b>受け入れます</b>。"],
        ["produce", "生産する、生み出す", "This factory produces eco-friendly cars.", "この工場は環境に優しい車を<b>生産している</b>。"],
        ["exist", "存在する", "Do you believe aliens exist?", "宇宙人は<b>存在する</b>と思いますか？"],
        ["express", "表現する", "It is important to express your feelings.", "自分の気持ちを<b>表現する</b>ことは大切だ。"],
        ["add", "加える", "Please add some sugar to my coffee.", "コーヒーに砂糖を少し<b>加えて</b>ください。"],
        ["avoid", "避ける", "You should avoid making the same mistake.", "同じ間違いをすることを<b>避ける</b>べきだ。"],
        ["marry", "結婚する", "They decided to marry next spring.", "彼らは来年の春に<b>結婚する</b>ことを決めた。"],
        ["protect", "保護する、守る", "We must protect endangered species.", "私たちは絶滅危惧種を<b>守ら</b>なければならない。"],
        ["affect", "影響を与える", "The weather can affect our mood.", "天気は私たちの気分に<b>影響を与える</b>ことがある。"],
        ["determine", "決定する、突き止める", "Diet and exercise determine your health.", "食事と運動があなたの健康を<b>決定する</b>。"],
        ["solve", "解決する", "He managed to solve the difficult puzzle.", "彼はその難しいパズルをなんとか<b>解いた</b>。"],
        ["contain", "含む", "This bottle contains fresh water.", "このボトルには新鮮な水が<b>入っている</b>。"],
        ["discuss", "話し合う", "We need to discuss the new plan.", "私たちは新しい計画について<b>話し合う</b>必要がある。"],
        ["ignore", "無視する", "Don't ignore the warning signs.", "警告のサインを<b>無視して</b>はいけない。"],
        ["guess", "推測する", "Can you guess how old he is?", "彼の年齢を<b>推測</b>できますか？"],
        ["exchange", "交換する", "They exchanged business cards.", "彼らは名刺を<b>交換した</b>。"],
        ["satisfy", "満足させる", "The meal was enough to satisfy his hunger.", "その食事は彼の空腹を<b>満たす</b>のに十分だった。"],
        ["complain", "不満を言う", "Customers often complain about the service.", "顧客はよくサービスについて<b>不満を言う</b>。"],
        ["achieve", "達成する", "She worked hard to achieve her goals.", "彼女は目標を<b>達成する</b>ために懸命に働いた。"],
        ["enable", "可能にする", "This software enables fast communication.", "このソフトウェアは迅速な通信を<b>可能にする</b>。"],
        ["intend", "意図する、〜するつもりだ", "I intend to study abroad next year.", "私は来年留学する<b>つもりだ</b>。"],
        ["obtain", "手に入れる、獲得する", "You need to obtain a visa to enter.", "入国するにはビザを<b>取得する</b>必要がある。"],
        ["divide", "分割する、分ける", "Let's divide the cake into four pieces.", "ケーキを4つのピースに<b>分け</b>ましょう。"],
        ["annoy", "イライラさせる", "The loud noise started to annoy me.", "大きな騒音が私を<b>イライラさせ</b>始めた。"],
        ["differ", "異なる", "Customs differ from country to country.", "習慣は国によって<b>異なる</b>。"],
        ["educate", "教育する", "Schools educate children for the future.", "学校は将来のために子供たちを<b>教育する</b>。"],
        ["borrow", "借りる", "Can I borrow your umbrella for a moment?", "傘を少しの間<b>借り</b>てもいいですか？"],
        ["invent", "発明する", "Thomas Edison invented the light bulb.", "トーマス・エジソンが電球を<b>発明した</b>。"],
        ["promote", "促進する、昇進させる", "The company wants to promote healthy living.", "その会社は健康的な生活を<b>促進し</b>たいと考えている。"],
        ["advice", "忠告、助言", "He gave me some useful advice.", "彼は私に役立つ<b>助言</b>をくれた。"],
        ["retire", "引退する、退職する", "He plans to retire at the age of 65.", "彼は65歳で<b>退職する</b>予定だ。"],
        ["permit", "許可する", "Smoking is not permitted inside the building.", "建物内での喫煙は<b>許可されて</b>いない。"],
        ["recommend", "勧める", "I highly recommend this restaurant.", "このレストランを強く<b>お勧めします</b>。"],
        ["apologize", "謝罪する", "You should apologize for being late.", "遅刻したことを<b>謝罪すべきだ</b>。"],
        ["inform", "知らせる", "Please inform us of any changes.", "変更があれば私たちに<b>知らせて</b>ください。"],
        ["oppose", "反対する", "Many citizens oppose the new tax plan.", "多くの市民がその新しい税制案に<b>反対している</b>。"],
        ["trust", "信頼する", "I trust him because he is honest.", "彼は正直なので、私は彼を<b>信頼している</b>。"],
        ["select", "選ぶ", "You can select your favorite color.", "好きな色を<b>選ぶ</b>ことができます。"],
        ["praise", "褒める", "The teacher praised her for the good work.", "先生は彼女の良い仕事を<b>褒めた</b>。"],
        ["handle", "扱う、処理する", "She knows how to handle difficult clients.", "彼女は気難しい顧客の<b>扱い方</b>を知っている。"],
        ["propose", "提案する、プロポーズする", "He proposed a new marketing strategy.", "彼は新しいマーケティング戦略を<b>提案した</b>。"],
        ["breathe", "呼吸する", "Take a deep breath and breathe out slowly.", "深呼吸をして、ゆっくり息を<b>吐いて</b>ください。"],
        ["criticize", "批判する", "It is easy to criticize others.", "他人を<b>批判する</b>のは簡単だ。"],
        ["overcome", "克服する", "She managed to overcome her fear of heights.", "彼女は高所恐怖症をなんとか<b>克服した</b>。"],
        ["possess", "所有する", "He possesses great musical talent.", "彼は素晴らしい音楽の才能を<b>持っている</b>。"],
        ["predict", "予測する", "Scientists cannot predict earthquakes accurately.", "科学者は地震を正確に<b>予測する</b>ことはできない。"],
        ["publish", "出版する、発表する", "They will publish the results next week.", "彼らは来週、結果を<b>発表する</b>。"],
        ["floating", "浮かんでいる", "I saw leaves floating on the river.", "川に葉が<b>浮かんでいる</b>のが見えた。"],
        ["recall", "思い出す、呼び戻す", "I cannot recall where I met him.", "彼にどこで会ったか<b>思い出せない</b>。"],
        ["explore", "探検する、調査する", "We love to explore old towns.", "私たちは古い街を<b>探検する</b>のが大好きだ。"],
        ["pretend", "〜のふりをする", "She pretended to be asleep.", "彼女は眠っている<b>ふりをした</b>。"],
        ["absorb", "吸収する", "Plants absorb water through their roots.", "植物は根から水を<b>吸収する</b>。"],
        ["resemble", "〜に似ている", "She closely resembles her mother.", "彼女は母親によく<b>似ている</b>。"],
        ["tear", "破く、引き裂く", "Be careful not to tear the paper.", "紙を<b>破か</b>ないように気をつけて。"],
        ["consume", "消費する", "Cars consume a lot of fuel.", "車は多くの燃料を<b>消費する</b>。"],
        ["compete", "競争する", "Athletes compete for the gold medal.", "アスリートたちが金メダルを目指して<b>競い合う</b>。"],
        ["quit", "辞める", "He decided to quit his job.", "彼は仕事を<b>辞める</b>ことに決めた。"],
        ["announce", "発表する、告知する", "The company will announce a new product soon.", "会社は間もなく新製品を<b>発表する</b>。"],
        ["react", "反応する", "How did he react to the news?", "彼はそのニュースにどう<b>反応しましたか</b>？"],
        ["wander", "歩き回る、彷徨う", "We wandered through the forest for hours.", "私たちは何時間も森の中を<b>歩き回った</b>。"],
        ["text", "本文、テキスト", "Please read the text carefully.", "<b>本文</b>を注意深く読んでください。"],
        ["generate", "生み出す、発生させる", "Windmills generate electricity.", "風車は電気を<b>生み出す</b>。"],
        ["score", "得点、スコア", "The final score was three to two.", "最終<b>スコア</b>は3対2だった。"],
        ["government", "政府", "The government announced a new policy.", "<b>政府</b>は新しい政策を発表した。"],
        ["knowledge", "知識", "Knowledge is power.", "<b>知識</b>は力なり。"],
        ["nation", "国家、国", "Leaders from every nation attended the meeting.", "あらゆる<b>国</b>の指導者がその会議に出席した。"],
        ["effort", "努力", "Success requires a lot of effort.", "成功には多くの<b>努力</b>が必要だ。"],
        ["period", "期間、時代", "He was away for a long period of time.", "彼は長<b>期間</b>留守にしていた。"],
        ["population", "人口", "The world population is growing rapidly.", "世界の<b>人口</b>は急速に増加している。"],
        ["purpose", "目的", "What is the purpose of your visit?", "あなたの訪問の<b>目的</b>は何ですか？"],
        ["behavior", "行動、振る舞い", "His behavior at the party was excellent.", "パーティでの彼の<b>振る舞い</b>は素晴らしかった。"],
        ["lack", "不足", "The project failed due to a lack of funds.", "資金<b>不足</b>のため、そのプロジェクトは失敗した。"],
        ["skill", "技術、スキル", "She has great communication skills.", "彼女は素晴らしいコミュニケーション<b>スキル</b>を持っている。"],
        ["quality", "質、品質", "We focus on the quality of our products.", "私たちは製品の<b>品質</b>に焦点を当てている。"],
        ["environment", "環境", "We must protect the natural environment.", "私たちは自然<b>環境</b>を守らなければならない。"],
        ["role", "役割", "She played an important role in the team.", "彼女はチームで重要な<b>役割</b>を果たした。"],
        ["attitude", "態度", "He has a positive attitude toward learning.", "彼は学習に対して前向きな<b>態度</b>をとっている。"],
        ["author", "著者、作家", "Who is the author of this novel?", "この小説の<b>著者</b>は誰ですか？"],
        ["research", "研究、調査", "Market research helps understand customers.", "市場<b>調査</b>は顧客を理解するのに役立つ。"],
        ["opportunity", "機会、チャンス", "Don't miss this great opportunity.", "この素晴らしい<b>機会</b>を逃さないで。"],
        ["source", "源、情報源", "The sun is our main source of energy.", "太陽は私たちの主なエネルギーの<b>源</b>だ。"],
        ["carbon", "炭素", "Trees absorb carbon dioxide.", "木々は二酸化<b>炭素</b>を吸収する。"],
        ["shape", "形、姿", "The cloud had the shape of a heart.", "その雲はハートの<b>形</b>をしていた。"],
        ["advantage", "利点、強み", "Being fluent in English is a big advantage.", "英語が堪能なことは大きな<b>強み</b>だ。"],
        ["method", "方法、手法", "This is the most efficient method of teaching.", "これが最も効率的な教授<b>法</b>だ。"],
        ["habit", "習慣、癖", "Smoking is a bad habit.", "喫煙は悪い<b>習慣</b>だ。"],
        ["detail", "詳細", "Please tell me the details of the plan.", "計画の<b>詳細</b>を教えてください。"],
        ["distance", "距離", "Long-distance relationships can be difficult.", "遠<b>距離</b>恋愛は難しい場合がある。"],
        ["crowd", "群衆、人混み", "A large crowd gathered to see the concert.", "コンサートを見るために大勢の<b>群衆</b>が集まった。"],
        ["instance", "例、事例", "For instance, you can use this app to learn.", "<b>例えば</b>、学ぶためにこのアプリを使うことができる。"],
        ["desire", "願望、欲望", "He has a strong desire to succeed.", "彼は成功したいという強い<b>願望</b>を持っている。"],
        ["standard", "基準、標準", "Our products meet international standards.", "当社の製品は国際<b>基準</b>を満たしている。"],
        ["task", "任務、仕事、タスク", "Completing this task will take some time.", "この<b>タスク</b>を完了するには少し時間がかかる。"],
        ["generation", "世代", "The younger generation loves technology.", "若い<b>世代</b>はテクノロジーが大好きだ。"],
        ["responsibility", "責任", "Taking care of a pet is a big responsibility.", "ペットの世話をするのは大きな<b>責任</b>を伴う。"],
        ["experiment", "実験", "The scientists conducted a chemical experiment.", "科学者たちは化学<b>実験</b>を行った。"],
        ["athlete", "アスリート、運動選手", "She is a professional athlete.", "彼女はプロの<b>アスリート</b>だ。"],
        ["decade", "10年間", "They have lived here for a decade.", "彼らはここに<b>10年間</b>住んでいる。"],
        ["loss", "損失、失うこと", "The company reported a huge financial loss.", "その会社は巨額の財務<b>損失</b>を報告した。"],
        ["fever", "熱、発熱", "He stayed in bed with a high fever.", "彼は高<b>熱</b>を出して寝込んでいた。"],
        ["theory", "理論", "His theory is supported by many facts.", "彼の<b>理論</b>は多くの事実によって裏付けられている。"],
        ["statement", "声明、発言", "The president issued an official statement.", "大統領は公式<b>声明</b>を出した。"],
        ["professor", "教授", "The professor explained the topic clearly.", "<b>教授</b>はそのトピックを分かりやすく説明した。"],
        ["function", "機能、役割", "What is the main function of this machine?", "この機械の主な<b>機能</b>は何ですか？"],
        ["surface", "表面", "The moon's surface is covered with dust.", "月の<b>表面</b>は塵で覆われている。"],
        ["envelope", "封筒", "Put the letter inside the envelope.", "手紙を<b>封筒</b>の中に入れなさい。"],
        ["organization", "組織、団体", "She works for an international organization.", "彼女は国際<b>機関</b>で働いている。"],
        ["policy", "政策、方針", "The school has a strict safety policy.", "その学校には厳格な安全<b>方針</b>がある。"],
        ["resource", "資源", "Water is a precious natural resource.", "水は貴重な天然<b>資源</b>だ。"],
        ["contrast", "対比、コントラスト", "There is a sharp contrast between the two.", "その2つの間には鮮やかな<b>対比</b>がある。"],
        ["flood", "洪水", "The heavy rain caused a massive flood.", "豪雨が大規模な<b>洪水</b>を引き起こした。"],
        ["mate", "仲間、連れ合い", "Penguins choose a mate for life.", "ペンギンは生涯の<b>伴侶</b>を選ぶ。"],
        ["goods", "商品、品物", "The store sells leather goods.", "その店は革<b>製品</b>を販売している。"],
        ["creature", "生き物", "The ocean is full of strange creatures.", "海は奇妙な<b>生き物</b>で満ちている。"],
        ["structure", "構造、建造物", "The DNA structure was discovered in 1953.", "DNA<b>構造</b>は1953年に発見された。"],
        ["tradition", "伝統", "It is a tradition to eat special food on New Year's Day.", "元日に特別な料理を食べるのは<b>伝統</b>だ。"],
        ["weight", "体重、重さ", "I am trying to lose weight.", "私は<b>体重</b>を減らそうとしている。"],
        ["charity", "慈善活動、チャリティ", "He donated a large sum of money to charity.", "彼は<b>慈善団体</b>に多額の資金を寄付した。"]
      ];

      let current = 0;
      let score = 0;
      let answered = false;
      let results = [];

      const jpTranslation = document.getElementById("japanese-translation");
      const targetHint = document.getElementById("target-word-hint");
      const question = document.getElementById("question");
      const input = document.getElementById("answerInput");
      const result = document.getElementById("result");
      const progress = document.getElementById("progress");
      const nextBtn = document.getElementById("nextBtn");
      const checkBtn = document.getElementById("checkBtn");

      function shuffleWords() {
        words.sort(() => Math.random() - 0.5);
      }

      function makeBlank(sentence, answer) {
        const escapedAnswer = answer.replace(/[-\/\\^$*+?.()|[\]{}]/g, '\\$&');
        // 四角い空欄（インライン要素）を埋め込む
        return sentence.replace(
          new RegExp(`\\b${escapedAnswer}\\b`, "i"),
          `<span class="blank-box" id="current-blank"></span>`
        ).replace(
          // 単語境界がない場合のフォールバック（in spite of等）
          new RegExp(escapedAnswer, "i"),
          `<span class="blank-box" id="current-blank"></span>`
        );
      }

      function loadQuestion() {
        const word = words[current];

        progress.textContent = `Q. ${current + 1} / ${words.length}`;
        jpTranslation.innerHTML = word[3]; // 日本語の例文訳（太字対応）
        targetHint.textContent = `単語の意味：${word[1]}`; // 単語自体のヒント
        question.innerHTML = makeBlank(word[2], word[0]); // 四角い空欄を埋め込んだ英文

        input.value = "";
        input.className = "";
        
        // 表示をリセット
        progress.style.display = "block";
        jpTranslation.style.display = "block";
        targetHint.style.display = "block";
        input.style.display = "block";
        result.style.display = "block";
        result.textContent = "";
        
        answered = false;

        checkBtn.style.display = "inline-block";
        nextBtn.style.display = "none";

        input.focus();
      }

      function checkAnswer() {
        if (answered) return;

        answered = true;

        const correct = words[current][0];
        const userTyped = input.value.trim();
        
        // 余分なスペースなどを無視して比較
        const isCorrect = userTyped.toLowerCase().replace(/\s+/g, ' ') === correct.toLowerCase().replace(/\s+/g, ' ');

        const blankBox = document.getElementById("current-blank");

        if (isCorrect) {
          score++;
          result.textContent = "⭕ 正解！";
          result.style.color = "#2ecc71";
          input.classList.add("correct-input");
          
          if (blankBox) {
            blankBox.textContent = correct;
            blankBox.classList.add("revealed", "correct");
          }
        } else {
          result.textContent = `❌ 不正解... （正解：${correct}）`;
          result.style.color = "#e74c3c";
          input.classList.add("wrong-input");

          if (blankBox) {
            blankBox.textContent = correct;
            blankBox.classList.add("revealed", "wrong");
          }
        }

        results.push({
          word: correct,
          jp: words[current][1],
          sentence: words[current][2],
          jpSentence: words[current][3],
          userAnswer: userTyped || "（未入力）",
          correct: isCorrect
        });

        checkBtn.style.display = "none";
        nextBtn.style.display = "inline-block";
        nextBtn.focus();
      }

      function showResults() {
        // クイズエリアの要素を隠す
        jpTranslation.style.display = "none";
        targetHint.style.display = "none";
        input.style.display = "none";
        checkBtn.style.display = "none";
        nextBtn.style.display = "none";
        result.style.display = "none";
        progress.style.display = "none";

        let html = `
          <div class="result-header">
            <h2>結果発表</h2>
            <div class="result-score">スコア：${score} / ${words.length}</div>
          </div>
          <div class="result-list">
        `;

        results.forEach((r, i) => {
          const bgClass = r.correct ? "correct-bg" : "wrong-bg";
          const textClass = r.correct ? "correct-text" : "wrong-text";
          const statusIcon = r.correct ? "⭕ 正解" : "❌ 不正解";

          html += `
            <div class="result-item ${bgClass}">
              <div class="result-word-info">
                <strong>${i + 1}. ${r.word}</strong> (${r.jp}) - ${statusIcon}
              </div>
              <div class="result-sentence" style="font-weight: bold; margin-bottom: 4px; color: #333;">
                ${r.jpSentence}
              </div>
              <div class="result-sentence" style="font-style: italic; margin-bottom: 8px;">
                ${r.sentence}
              </div>
              <div class="user-answer ${textClass}">
                あなたの回答: ${r.userAnswer}
              </div>
            </div>
          `;
        });

        html += `
          </div>
          <div class="retry-box">
            <button id="retryBtn" class="action-btn">もう一度挑戦する</button>
          </div>
        `;

        question.innerHTML = html;

        // 「もう一度挑戦する」ボタンのイベントを設定
        document.getElementById("retryBtn").addEventListener("click", resetQuiz);
      }

      function resetQuiz() {
        current = 0;
        score = 0;
        answered = false;
        results = [];
        
        shuffleWords();
        loadQuestion();
      }

      function nextQuestion() {
        current++;

        if (current >= words.length) {
          showResults();
          return;
        }

        loadQuestion();
      }

      checkBtn.addEventListener("click", checkAnswer);
      nextBtn.addEventListener("click", nextQuestion);

      input.addEventListener("keydown", (e) => {
        if (e.key === "Enter") {
          e.preventDefault();
          if (!answered) {
            checkAnswer();
          } else {
            nextQuestion();
          }
        }
      });

      // フルスクリーン切り替え
      document.getElementById("fullscreenBtn").addEventListener("click", async () => {
        if (!document.fullscreenElement) {
          try {
            await document.documentElement.requestFullscreen();
          } catch (err) {
            console.error(err);
          }
        } else {
          await document.exitFullscreen();
        }
      });

      // クイズ開始の初期化
      shuffleWords();
      loadQuestion();
    });
  </script>
</body>
</html>

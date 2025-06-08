# モデリングガイドライン修正案

## 3.寸法読み取り原則に追加する修正項目

### 3.15 寸法補助線間隔による相対検証システム（新規追加）

#### 基本原理
```yaml
relative_dimension_verification:
  principle: "寸法補助線の間隔は寸法値に比例する"
  method: "確実な寸法を基準として他の寸法を相対検証"
  critical_importance: "数値誤読防止の最終チェック手段"
```

#### 相対検証手順
```yaml
verification_procedure:
  step1_reference_establishment:
    action: "最も確実に読み取れる寸法を基準値として設定"
    examples: 
      - "明確に記載された小さな寸法（例：7mm、5mm）"
      - "重複して記載されている寸法"
      - "標準的な寸法値（例：φ記号付きの寸法）"
    
  step2_interval_measurement:
    action: "基準寸法の補助線間隔を測定単位として設定"
    method: "画面上での物理的な間隔を目視で計測"
    note: "正確な測定より相対的な比較を重視"
    
  step3_proportional_verification:
    action: "疑問のある寸法の補助線間隔を基準と比較"
    calculation: "疑問寸法の間隔 ÷ 基準寸法の間隔 × 基準寸法値"
    example: "間隔が1.7倍 × 7mm = 12mm程度"
    
  step4_plausibility_check:
    action: "計算結果と読み取り値の妥当性を判断"
    criteria:
      - "工学的に妥当な寸法値か"
      - "他の寸法との整合性があるか"  
      - "記号（φ等）との整合性があるか"
```

#### 誤読パターンと対策
```yaml
common_misreading_patterns:
  digit_confusion:
    pattern: "412 vs φ12の混同"
    cause: "φ記号の見落としと数字の誤読"
    prevention: "記号の存在確認と補助線間隔の相対検証"
    
  scale_misjudgment:
    pattern: "大きな数値への思い込み"
    cause: "複雑な部品への期待バイアス"
    prevention: "他寸法との比例関係による現実性チェック"
    
  unit_confusion:
    pattern: "mm単位での非現実的な大寸法"
    cause: "単位への注意不足"
    prevention: "実物サイズでの妥当性確認"
```

#### 強制検証ポイント
```yaml
mandatory_verification_triggers:
  large_dimension_detection:
    trigger: "100mm以上の寸法を読み取った場合"
    action: "必ず相対検証を実施"
    reason: "大寸法は誤読の可能性が高い"
    
  aspect_ratio_anomaly:
    trigger: "縦横比が10:1を超える場合"
    action: "全寸法の再読み取りを実施"
    reason: "極端なアスペクト比は通常設計では稀"
    
  symbol_dimension_mismatch:
    trigger: "記号（φ等）と数値の不整合"
    action: "記号周辺の詳細再確認"
    reason: "記号は寸法の意味を明確化する重要な情報"
```

### 3.16 数値認識エラーの防止システム（新規追加）

#### 文字・数字認識の強化
```yaml
character_recognition_enhancement:
  symbol_first_principle:
    rule: "数値読み取り前に必ず記号（φ、C、R等）を確認"
    method: "記号→数値の順序で読み取り"
    benefit: "記号が数値の妥当性判断の手がかりとなる"
    
  context_verification:
    rule: "読み取った数値が文脈で妥当かを必ず確認"
    checks:
      - "φ記号があるのに100mm超の値は疑問"
      - "面取りC記号で10mm超の値は疑問"  
      - "位置寸法が全体寸法を超えることはない"
    
  visual_comparison:
    rule: "同じ数字が他箇所にあれば文字形状を比較"
    method: "フォント特徴による文字識別の確認"
    example: "1と4、2と7の判別"
```

#### デジタル認識バイアスの排除
```yaml
digital_bias_elimination:
  precision_illusion:
    problem: "デジタル図面だから全て正確に読めるという錯覚"
    reality: "表示解像度や文字サイズによる読み取り限界"
    approach: "アナログ図面と同様の慎重さで読み取り"
    
  font_variation_awareness:
    problem: "CADソフトによる文字フォントの違い"
    reality: "1と4、6と9等の判別困難な場合がある"
    approach: "文脈と相対検証による確認"
```

## 4.フィーチャー識別に追加する修正項目

### 4.6 記号と数値の統合判定システム（新規追加）

#### φ記号の正確な読み取り
```yaml
phi_symbol_reading:
  symbol_detection:
    priority: "数値読み取りより記号検出を優先"
    method: "φ記号の存在を先に確認してから数値を読む"
    verification: "φ記号がある場合は円・穴・円柱を想定"
    
  dimension_plausibility:
    small_holes: "φ3～φ20程度が一般的"
    large_holes: "φ50超は特殊用途でない限り稀"
    warning: "φ100超の読み取りは必ず再確認"
    
  context_consistency:
    rule: "φ寸法は他図面で円形要素として確認可能"
    verification: "正面図または上面図で対応する円形を確認"
    cross_check: "円形要素のサイズとφ値の一致確認"
```

## 11.エラー対処システムに追加する修正項目

### 11.4 寸法読み取り品質保証システム（新規追加）

#### 相対検証の義務化
```yaml
relative_verification_mandate:
  trigger_conditions:
    - "初回読み取り完了時"
    - "大寸法（50mm超）検出時"  
    - "アスペクト比異常時"
    - "記号付き寸法読み取り時"
    
  verification_procedure:
    step1: "基準寸法（7mm等の確実な値）を選定"
    step2: "疑問寸法の補助線間隔を基準と比較"
    step3: "比例計算による妥当性確認"
    step4: "工学的常識による最終判定"
    
  acceptance_criteria:
    - "相対計算値と読み取り値の差が20%以内"
    - "工学的に妥当な寸法範囲内"
    - "他図面での形状要素と整合"
```

#### エラー学習システム
```yaml
error_learning_system:
  common_error_database:
    "412_vs_phi12":
      description: "φ12をφ412と誤読"
      cause: "記号見落とし＋数値誤認"
      prevention: "φ記号優先確認＋相対検証"
      
    "scale_misinterpretation":
      description: "実際の10倍の寸法で読み取り"
      cause: "補助線間隔の誤判断"
      prevention: "基準寸法との比例確認"
      
  prevention_checklist:
    before_reading:
      - "[ ] 記号（φ、C、R等）の存在確認"
      - "[ ] 基準寸法の選定完了"
      - "[ ] 相対検証の準備完了"
      
    during_reading:
      - "[ ] 記号→数値の順序で読み取り"
      - "[ ] 大寸法は即座に相対検証"
      - "[ ] 工学的妥当性の常時確認"
      
    after_reading:
      - "[ ] 全寸法の相対検証完了"
      - "[ ] アスペクト比の妥当性確認"
      - "[ ] 記号と数値の整合性確認"
```

## 実装上の重要ポイント

### 即座に適用すべき改善項目
1. **相対検証の義務化**: 全ての寸法読み取りで実施
2. **記号優先読み取り**: φ、C、R等の記号を数値より先に確認
3. **工学的妥当性チェック**: 読み取り値の現実性を常時確認
4. **補助線間隔比較**: 確実な寸法を基準とした相対測定

### 長期的な改善項目
1. **エラーパターンDB構築**: 典型的誤読パターンの体系化
2. **自動検証システム**: 読み取り値の妥当性を自動チェック
3. **訓練プログラム強化**: 相対検証スキルの反復練習
4. **品質管理指標**: 寸法読み取り精度の定量的評価

この修正により、今回のような「φ12を412と誤読」するエラーを防止できると考えられます。

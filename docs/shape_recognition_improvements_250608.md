# 基本形状特定プロセスの抜本的改善

## 1. 現状ナレッジの根本的問題点

### 1.1 過度な単純化の問題
```yaml
# 【問題のある現状】
basic_shapes:
  rectangular_prism:
    all_views: "矩形"
    description: "直方体"
```

**問題点**：
- 複合形状（複数の基本形状の組み合わせ）を想定していない
- 単一の基本形状でない場合の分析方法がない
- 実際の部品は複合形状が多いのに対応できない

### 1.2 要素分解手順の欠如
**現状の不備**：
- 各図面で「何個の形状要素があるか」を数える手順がない
- 「実線」「破線」の意味分析が不十分
- 図面間での要素対応関係の確認方法が不明確

## 2. 抜本的改善：段階的形状認識システム

### 2.1 要素分解による形状認識（新規追加）

#### **段階1：各図面の要素分解**
```yaml
element_decomposition_process:
  step1_element_counting:
    method: "各図面で独立した形状要素の個数を数える"
    procedure:
      front_view:
        action: "実線で囲まれた独立領域を特定"
        example: "矩形1個 + 円形1個 = 2要素"
        note: "重なりや内包関係も考慮"
      
      side_view:
        action: "同様に独立領域を特定"
        example: "矩形2個（積み重なり） = 2要素"
        verification: "φ記号による円形要素の確認"
      
      top_view:
        action: "同様に独立領域を特定"
        note: "要素が見えない場合もある"
  
  step2_line_type_analysis:
    real_line_meaning:
      definition: "その図面から見える実際の境界線"
      implication: "形状が手前に存在している"
      example: "正面図の円形実線 → 円柱の正面投影"
    
    dashed_line_meaning:
      definition: "隠れた部分の境界線"
      implication: "形状が奥に存在している"
      example: "破線の円 → 向こう側の穴や円柱"
    
    center_line_meaning:
      definition: "対称軸や回転軸を示す"
      implication: "円形や対称形状の存在を示唆"
      note: "形状そのものではない"
  
  step3_element_classification:
    geometric_classification:
      rectangular_elements: "直方体、角柱、板状部品"
      circular_elements: "円柱、円錐、球、穴"
      complex_elements: "複数要素の組み合わせ"
    
    spatial_relationship:
      stacked: "要素が上下に積み重なっている"
      side_by_side: "要素が横に並んでいる"
      embedded: "一方の要素が他方に埋め込まれている"
      intersecting: "要素が互いに貫通している"
```

#### **段階2：図面間対応関係の確認**
```yaml
cross_view_correspondence:
  element_matching_process:
    step1_element_inventory:
      action: "各図面の要素リストを作成"
      format: "図面名：要素1（形状・サイズ）、要素2（形状・サイズ）..."
      example: 
        front_view: "矩形（19×30）、円形（φ12）"
        side_view: "矩形1（Z×5）、矩形2（φ12×4.5）"
    
    step2_correspondence_check:
      method: "同一要素が他の図面でどう見えるかを確認"
      verification_points:
        size_consistency: "対応する寸法が一致するか"
        position_consistency: "相対位置が整合するか"
        projection_logic: "投影関係が論理的に正しいか"
      
      example_analysis:
        front_circle_vs_side_rectangle:
          question: "正面図の円形φ12 vs 右側面図の矩形φ12×4.5"
          verification: "位置関係、サイズ、投影方向の整合性"
          conclusion: "同一要素（円柱）の異なる投影"
    
    step3_shape_synthesis:
      rule: "全図面で整合する3D形状を推定"
      method: "各要素の3D形状を特定し、組み合わせ方を決定"
      verification: "推定した3D形状が全図面と一致するか確認"
```

### 2.2 複合形状の系統的分析（新規追加）

#### **複合形状認識システム**
```yaml
composite_shape_analysis:
  identification_triggers:
    multiple_elements_per_view:
      trigger: "1つの図面に複数の独立要素がある"
      example: "正面図に矩形と円形が両方存在"
      action: "複合形状の可能性を検討"
    
    dimensional_inconsistency:
      trigger: "単一基本形状では説明できない寸法関係"
      example: "高さが段階的に変化している"
      action: "積層構造や段差構造を検討"
    
    feature_symbols:
      trigger: "φ、R、面取りなどの記号が基本形状に追加"
      example: "矩形にφ記号が併記"
      action: "基本形状＋加工フィーチャーの組み合わせ"
  
  composite_types:
    stacked_composition:
      description: "複数の基本形状が積み重なった構造"
      identification: "右側面図で高さ方向に複数の矩形"
      example: "ベース直方体 + 上部円柱"
      modeling_approach: "下から順番に要素を追加"
    
    side_by_side_composition:
      description: "複数の基本形状が横に並んだ構造"
      identification: "平面図で幅方向に複数の形状"
      example: "L字型、T字型の構造"
      modeling_approach: "基本形状の結合演算"
    
    embedded_composition:
      description: "一方の形状に他方が埋め込まれた構造"
      identification: "破線による内部形状の表現"
      example: "穴あき部品、中空構造"
      modeling_approach: "基本形状からの減算演算"
  
  analysis_procedure:
    step1_primary_shape:
      action: "最も大きな（基本となる）形状要素を特定"
      criteria: "寸法が最大、全図面で確認できる"
      example: "19×30×5の直方体部分"
    
    step2_secondary_shapes:
      action: "追加される形状要素を特定"
      criteria: "基本形状に付加される要素"
      example: "φ12×4.5の円柱部分"
    
    step3_spatial_relationship:
      action: "要素間の3D配置関係を特定"
      method: "各図面での位置関係から推定"
      verification: "全図面で位置関係が整合するか確認"
```

### 2.3 形状確定の検証システム（新規追加）

#### **確定前検証チェックリスト**
```yaml
shape_verification_system:
  before_shape_conclusion:
    element_completeness_check:
      - "[ ] 全図面の全要素を特定した"
      - "[ ] 各要素の3D形状を決定した"
      - "[ ] 要素間の配置関係を明確化した"
    
    consistency_verification:
      - "[ ] 推定形状が正面図と一致する"
      - "[ ] 推定形状が右側面図と一致する"
      - "[ ] 推定形状が上面図と一致する"
      - "[ ] 寸法の整合性を確認した"
    
    logic_check:
      - "[ ] 投影関係が論理的に正しい"
      - "[ ] 製造可能な形状である"
      - "[ ] 機能的に合理的である"
  
  common_error_prevention:
    oversimplification:
      error: "複合形状を単一形状と誤認"
      prevention: "必ず各図面の要素数を確認"
      example: "矩形＋円形を単なる直方体と誤認しない"
    
    line_type_misinterpretation:
      error: "実線と破線の意味を混同"
      prevention: "線種の意味を必ず確認"
      example: "実線の円は円柱、破線の円は穴"
    
    projection_confusion:
      error: "図面間の対応関係を間違える"
      prevention: "寸法と位置の整合性を必ず確認"
      example: "正面図の円と側面図の矩形の対応確認"
  
  error_recovery_procedure:
    detection_triggers:
      - "推定形状と図面が一致しない"
      - "寸法に矛盾がある"
      - "製造不可能な形状になっている"
    
    recovery_steps:
      step1: "要素分解からやり直し"
      step2: "各図面を独立して再分析"
      step3: "対応関係を慎重に再確認"
      step4: "別の形状仮説を検討"
```

## 3. 実践的な形状特定手順（完全改訂）

### 3.1 新しい形状特定ワークフロー

#### **フェーズ1：要素の系統的抽出**
```yaml
phase1_systematic_extraction:
  front_view_analysis:
    step1: "実線で囲まれた領域をすべて特定"
    step2: "各領域の形状（矩形/円形/その他）を分類"
    step3: "各領域のサイズを確認"
    step4: "重なりや包含関係を記録"
    
    example_output:
      - "要素1：矩形（19×30）"
      - "要素2：円形（φ12）、要素1の上に配置"
  
  side_view_analysis:
    step1: "同様の要素抽出を実施"
    step2: "φ記号による円形要素の確認"
    step3: "積層関係や並列関係を記録"
    
    example_output:
      - "要素1：矩形（Z×5）、下部"
      - "要素2：矩形（φ12×4.5）、上部、円形マーク付き"
  
  top_view_analysis:
    step1: "同様の分析を実施"
    step2: "見えない要素がある場合は記録"
    
    note: "上面図で要素が見えない場合もある"
```

#### **フェーズ2：対応関係の論理的確認**
```yaml
phase2_correspondence_verification:
  matching_algorithm:
    step1_size_matching:
      rule: "同じサイズの要素は同一要素の可能性が高い"
      example: "正面図φ12 ↔ 側面図φ12×4.5"
    
    step2_position_matching:
      rule: "相対位置が整合する要素は同一要素"
      verification: "投影線による位置確認"
    
    step3_projection_logic:
      rule: "投影方向で形状が変わる論理性確認"
      example: "円柱：正面から見ると円、側面から見ると矩形"
  
  correspondence_table_creation:
    format: "要素ID | 正面図 | 側面図 | 上面図 | 3D形状推定"
    example:
      - "要素A | 矩形19×30 | 矩形Z×5 | 矩形19×Z | 直方体19×30×5"
      - "要素B | 円形φ12 | 矩形φ12×4.5 | 円形φ12 | 円柱φ12×4.5"
```

#### **フェーズ3：3D形状の確定**
```yaml
phase3_3d_shape_determination:
  individual_element_shapes:
    method: "各要素の3D形状を個別に確定"
    verification: "全図面での見え方と一致するか確認"
  
  spatial_relationship:
    method: "要素間の3D配置関係を確定"
    types: ["積層", "並列", "埋込", "貫通"]
    verification: "図面の位置関係と一致するか確認"
  
  composite_shape_assembly:
    method: "個別要素を組み合わせて最終形状を決定"
    verification: "組み合わせ結果が全図面と一致するか確認"
```

## 4. エラー防止のための強化策

### 4.1 必須確認項目の追加

#### **形状特定時の必須チェックポイント**
```yaml
mandatory_checkpoints:
  before_starting:
    - "[ ] 各図面で要素数を数えた"
    - "[ ] 実線・破線・中心線を区別した"
    - "[ ] φ記号やその他の形状記号を確認した"
  
  during_analysis:
    - "[ ] 複数要素がある場合は複合形状を検討している"
    - "[ ] 各要素の対応関係を確認している"
    - "[ ] 推測ではなく論理的根拠で判断している"
  
  before_conclusion:
    - "[ ] 推定形状が全図面と一致することを確認した"
    - "[ ] 寸法の整合性を確認した"
    - "[ ] 製造可能性を確認した"
  
error_prevention_rules:
  single_shape_assumption:
    prohibition: "図面を見て即座に「直方体」などと決めつけることを禁止"
    requirement: "必ず要素分解から開始する"
  
  line_type_neglect:
    prohibition: "実線・破線の区別を無視することを禁止"
    requirement: "線種の意味を必ず確認する"
  
  projection_skip:
    prohibition: "図面間の対応確認を省略することを禁止"
    requirement: "全図面での整合性を必ず確認する"
```

この改善により、複合形状の正確な認識と、系統的な形状特定プロセスが確立されます。特に、「要素分解 
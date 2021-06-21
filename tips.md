> gradient_accumulation_steps

如果显存不足，我们可以通过gradient_accumulation_steps梯度累计来解决。  
假设原来的batch size=10,数据总量为1000，那么一共需要100train steps，同时一共进行100次梯度更新。  
若是显存不够，我们需要减小batch size。可以设置gradient_accumulation_steps=2，那么新的batch size=10/2=5。  
这时我们需要运行两个batch，才能处理10条数据。梯度更新的次数不变为100次，那么我们的train steps=200 。


> convert_examples_to_features

```

# 原始词序列
tokens_a_org:['A', 'boy', 'is', 'jumping', 'on', 'skateboard', 'in', 'the', 'middle', 'of', 'a', 'red', 'bridge', '.']
tokens_b_org:['The', 'boy', 'skates', 'down', 'the', 'sidewalk', '.']

# 分词之后的子词序列
token_a:['a', 'boy', 'is', 'jumping', 'on', 'skate', '##board', 'in', 'the', 'middle', 'of', 'a', 'red', 'bridge', '.']
token_b:['the', 'boy', 'skate', '##s', 'down', 'the', 'sidewalk', '.']

# 原始完整词下标
tok_to_orig_index_a:[0, 1, 2, 3, 4, 5, 6, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]   # 第一个位置'[CLS]'
tok_to_orig_index_b:[0, 1, 2, 2, 3, 4, 5, 6, 7]

# 合并a、b两部分的原始完整token下标序列
over_tok_to_orig_index:[0, 1, 2, 3, 4, 5, 6, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 18, 19, 20, 21, 22, 23]


# 完整输入序列: "[CLS]" a "[SEP]" b "[SEP]"
tokens:['[CLS]', 'a', 'boy', 'is', 'jumping', 'on', 'skate', '##board', 'in', 'the', 'middle', 'of', 'a', 'red', 'bridge', '.', '[SEP]', 'the', 'boy', 'skate', '##s', 'down', 'the', 'sidewalk', '.', '[SEP]']


#  tokens 转为 id
input_ids:[101, 1037, 2879, 2003, 8660, 2006, 17260, 6277, 1999, 1996, 2690, 1997, 1037, 2417, 2958, 1012, 102, 1996, 2879, 17260, 2015, 2091, 1996, 11996, 1012, 102]

#原始词的下标
orig_to_token_split_idx: [(0, 0), (1, 1), (2, 2), (3, 3), (4, 4), (5, 5), (6, 7), (8, 8), (9, 9), (10, 10), (11, 11), (12, 12), (13, 13), (14, 14), (15, 15), (16, 16), (17, 17), (18, 18), (19, 20), (21, 21), (22, 22), (23, 23), (24, 24), (25, 25)]

orig_to_token_split_idx: raw_words对应的其sub_words的所有下标,
比如： raw_words=skateboard,  sub_words=['skate', '##board'],  分词序列的下标：[6, 7] （在tok_to_orig_index_a中的位置下标）
所以，skateboard对应的为(6, 7)
```
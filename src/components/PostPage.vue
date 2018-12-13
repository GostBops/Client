<template>
  <section class="post-view">
    <div v-if="!content">Loading..</div>
    <article
      v-if="content"
      v-html="htmlFromMarkdown"/>
  </section>
</template>

<script>
import marked from 'marked'
import Prism from 'prismjs'
import fm from 'front-matter'

let data = `---\ntitle: 'LeetCode | [Week 14] 23. Merge k Sorted Lists'\n---\n
# LeetCode 23. Merge k Sorted Lists

## 问题描述

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

## Example

\`\`\`cpp
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
\`\`\`

## 题解

不凡想想，用最粗暴的方式，应该怎么做？首先同时扫描这k个分组，然后一一比较指针所指元素大小，然后选最小的填入结果数组，按上述过程直至所有分组扫描完成，那么这样做的复杂度有多高呢？首先，粗略地估计一下，对每个结果数组中的元素，大概需要k次比较，那么如果有k个分组，每个分组有n个元素，则时间复杂度为O(k<sup>2</sup>n)，空间复杂度为O(nk)，那有没有更好的算法呢？

思路其实很简单，实际上跟归并排序一样，回顾一下归并排序是怎么操作的：

1. 首先将数组每次对半分，直至每组元素个数为1
2. 将上述元素个数为1的分组两两合并，合并后的更长分组再两两合并，直至分组元素个数等于原数组元素个数
3. 合并过程即分别扫描两个数组，每次比较指针所指元素大小，小的放入暂存数组，然后继续扫描，直至两个数组扫描完成

那么实际上，这个题利用分治法，实际上类似与归并排序的合并过程。以题中示例，简述一下这个过程：

\`\`\`cpp
---------------------------------------------------------------------------------------
round 0: 0: 1->4->5, 1: 1->3->4
---------------------------------------------------------------------------------------
# 0
0: 1->4->5
1: 1->3->4
# 1 (tmp: 1)
0: 4->5
1: 1->3->4
# 2 (tmp: 1->1)
0: 4->5
1: 3->4
# 3 (tmp: 1->1->3)
0: 4->5
1: 4
...
# 6 (tmp: 1->1->3->4->4->5)

---------------------------------------------------------------------------------------
round 1: 0: 1->1->3->4->4->5, 1: 2->6
---------------------------------------------------------------------------------------
# 0
0: 1->1->3->4->4->5
1: 2->6
# 1 (tmp: 1)
0: 1->3->4->4->5
1: 2->6
# 2 (tmp: 1->1)
0: 3->4->4->5
1: 2->6
# 3 (tmp: 1->1->2)
0: 3->4->4->5
1: 6
...
# 8 (tmp: 1->1->2->3->4->4->5->6)
\`\`\`

稍微与归并排序合并过程不同的是这道题的分组长度并不相似，也就是说，有可能一个分组为10，另一个分组为1。

## Code

\`\`\`cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
private:
    ListNode* merge(vector<ListNode*>& lists, int begin, int end) {
      if (begin == end) return lists[begin];

      int mid = (begin + end) / 2;
      ListNode *left  = merge(lists, begin, mid);
      ListNode *right = merge(lists, mid + 1, end);
      ListNode *result = NULL, *current = NULL;

      while (left != NULL && right != NULL) {
        if (left->val < right->val) {
          if (result == NULL) {
            result = left;
            current = left;
            left = left->next;
            continue;
          }
          current->next = left;
          current = current->next;
          left = left->next;
        } else {
          if (result == NULL) {
            result = right;
            current = right;
            right = right->next;
            continue;
          }
          current->next = right;
          current = current->next;
          right = right->next;
        }
      }

      while (left != NULL) {
        if (result == NULL) {
          result = left;
          current = left;
          left = left->next;
          continue;
        }
        current->next = left;
        current = current->next;
        left = left->next;
      }

      while (right != NULL) {
        if (result == NULL) {
          result = right;
          current = right;
          right = right->next;
          continue;
        }
        current->next = right;
        current = current->next;
        right = right->next;
      }

      return result;
    }
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
      if (lists.size() == 0) return NULL;
      return merge(lists, 0, lists.size() - 1);
    }
};
\`\`\`

## 复杂度

假设有k个含有n个元素的分组，在合并的过程中，每一趟我们需要遍历nk个元素，同时需要nk的空间将结果暂存起来，因为有k个分组，所以我们共需要 log<sub>2</sub>k 趟，所以总的时间复杂度为 nklog<sub>2</sub>k，比起上面说的nk<sup>2</sup>要好一点，而空间复杂度为 nk，因为我们每一趟要讲中间结果存起来。

`

const renderer = new marked.Renderer()

renderer.heading = (text, level) => {
  const slug = text.replace(/<(?:.|\n)*?>/gm, '').toLowerCase().replace(/[\s\n\t]+/g, '-')
  return `<h${level} id="${slug}">${text}</h${level}>`
}

renderer.code = (code, lang) => {
  console.log(lang)
  if (lang === 'c' || lang === 'cpp') lang = 'clike'
  const highlight = Prism.highlight(code, Prism.languages[lang] || Prism.languages.javascript)
  return `<pre class="code-block"><code class="lang-${escape(lang, true)}">${highlight}</code></pre>`
}

marked.setOptions({
  renderer,
  breaks: true,
  gfm: true
})

export default {
  name: 'PostPage',

  data () {
    return {
      title: '',
      content: ''
    }
  },

  computed: {
    htmlFromMarkdown () {
      return marked(this.content)
    }
  },

  created () {
    this.loadPost()
  },

  methods: {
    loadPost () {
      // pretend get content from server
      // actually you should get 'data' from server 
      const content = fm(data)
      console.log(content)
      this.content = content.body
      this.title = content.attributes.title
    }
  }
}
</script>

<style>
@import "../assets/prism.css";

.code-block {
  background-color: #f5f2f0;
  padding: 5px;
}
</style>

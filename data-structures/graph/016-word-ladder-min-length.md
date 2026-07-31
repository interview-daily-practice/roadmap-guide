# Word ladder

Approach: Generate 1 char diff words and check if it is present in set then only add to queue and remove from set. 

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> words = new HashSet<>(wordList);
        if (!words.contains(endWord)) {
            return 0;
        }
        Set<String> v = new HashSet<>();
        int len = 0;
        Queue<String> q = new LinkedList<>();
        q.add(beginWord);
        v.add(beginWord);
        words.remove(beginWord);

        boolean endFound = false;
        while (!q.isEmpty()) {
            int size = q.size();
            len++;
            for (int i = 0; i < size; i++) {
                String current = q.poll();
                if (current.equals(endWord)) {
                    endFound = true;
                    return len;
                }

                char[] original = current.toCharArray();
                for (int ind = 0; ind < current.length(); ind++) {
                    char temp = original[ind];
                    for (int ic = 0; ic < 26; ic++) {
                        char c = (char) (ic + 97);
                        original[ind] = c;
                        String str = new String(original);
                        if (!words.contains(str)) {
                            continue;
                        }
                        q.add(str);
                        words.remove(str);
                    }
                    original[ind] = temp;
                }

            }
        }
        return 0;
    }

// not using this
    private boolean isOneCharDiff(String word1, String word2) {
        char[] ch1 = word1.toCharArray();
        char[] ch2 = word2.toCharArray();
        int diffCount = 0;
        for (int i = 0; i < ch1.length; i++) {
            if (ch1[i] != ch2[i]) {
                diffCount++;
            }
            if (diffCount > 1) {
                return false;
            }
        }

        return diffCount == 1;
    }
}
```

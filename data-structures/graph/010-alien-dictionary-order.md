# 269 Alien Dictionary Order

[Try On GFG](https://www.geeksforgeeks.org/problems/alien-dictionary/1)

```java
class Solution {
    public String findOrder(String[] words) {
        // code here
        return alienOrder(words);
    }
    
       public String alienOrder(String[] words) {
        Map<Character, Set<Character>> graph = new HashMap<>();

        // Step 1: character vs empty set
        for (String word : words) {
            for (Character c : word.toCharArray()) {
                graph.putIfAbsent(c, new HashSet<>());
            }
        }

        // Step 2: Adjacent word comparison

        for (int i = 0; i < words.length - 1; i++) {
            String word1 = words[i];
            String word2 = words[i + 1];

            int minLen = Math.min(word1.length(), word2.length());

            boolean mismatchFound = false;

            // step 2.1 mismatch found -> build edge of graph
            for (int charIndex = 0; charIndex < minLen; charIndex++) {
                if (word1.charAt(charIndex) != word2.charAt(charIndex)) {

                    graph.get(word1.charAt(charIndex)).add(word2.charAt(charIndex));

                    mismatchFound = true;
                    break;
                }
            }

            // step 2.2 invalid use case [abc,ab] no match found word1 > word2
            if (!mismatchFound && word1.length() > word2.length()) {
                return "";
            }
        }

        // Step3: dfs->cycle->topoSort: as we have graph build now. so lets do toposort
        Map<Character, Integer> state = new HashMap<>();
        StringBuffer topoResult = new StringBuffer();
        for (Character c : graph.keySet()) {
            if (!state.containsKey(c)) {
               boolean flag = dfs(graph, state, c, topoResult);
               if(flag) return "";
            }
        }

        return topoResult.reverse().toString();

    }


    private boolean dfs(Map<Character, Set<Character>> graph,
                        Map<Character, Integer> state,
                        Character character,
                        StringBuffer topoResult
    ) {

        if (state.get(character)!=null && state.get(character) == 1) return true;
        if (state.get(character)!=null && state.get(character) == 2) return false;

        state.put(character, 1);

        for (Character c : graph.get(character)) {
            if (dfs(graph, state, c, topoResult)) {
                return true;
            }
        }

        state.put(character, 2);

        topoResult.append(character);

        return false;

    }
}
```

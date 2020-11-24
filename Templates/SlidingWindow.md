Sliding window based problems are easy to identify if we practice a few.
Problems such as finding longest substring or shortest substring with some contraints are mostly based on sliding window.

Listing down 11 sliding window based problems which all follow same kind of approach with minor tweaks:

https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/
https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/
https://leetcode.com/problems/replace-the-substring-for-balanced-string/
https://leetcode.com/problems/max-consecutive-ones-iii/
https://leetcode.com/problems/subarrays-with-k-different-integers/
https://leetcode.com/problems/fruit-into-baskets/
https://leetcode.com/problems/get-equal-substrings-within-budget/
https://leetcode.com/problems/longest-repeating-character-replacement/
https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/
https://leetcode.com/problems/minimum-size-subarray-sum/
https://leetcode.com/problems/sliding-window-maximum/
Basic template of such problems is basically 3 steps.

Step1: Have a counter or hash-map to count specific array input and keep on increasing the window toward right using outer loop.
Step2: Have a while loop inside to reduce the window side by sliding toward right. Movement will be based on constraints of problem. Go through few examples below
Step3: Store the current maximum window size or minimum window size or number of windows based on problem requirement.

Sharing sample solutions for some to understand above steps.

Consider: https://leetcode.com/problems/max-consecutive-ones-iii/

		class Solution {
		public:
			int longestOnes(vector<int>& A, int K) {
				
				int count = 0, i = 0, j = 0;
				int res = INT_MIN;
				
				for (i = 0; i < A.size(); i++) {
					if (A[i] == 0) count++;  /* have some hashmap or counter */
					
					/* Loop inside for to reduce the window size based on constraint */
					while (count > K && j < A.size()) {
						if (A[j] == 0)
							count--;
						j++;
					}
					
					/* get the maximum value of the window which satisfies the constraint */
					res = max(res, i-j+1);
				}
				
				return res == INT_MIN ? ((count <= K) ? A.size() : 0) : res;
			}
		};
Consider: https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/

		class Solution {
		public:
			int numberOfSubstrings(string s) {
				
				int n = s.length();
				
				vector<int>count = {0, 0, 0};
				int i = 0, j = 0, res = 0, ans = 0;
				
				for (i = 0; i < n; i++) {
					count[s[i]-'a']++;     /* have some hashmap or counter */
					
					/* Loop inside for to reduce the window size based on constraint */
					while (j < n && count[0] && count[1] && count[2]) {
						ans++;
						count[s[j]-'a']--;
						j++;
					}
					
					/* Count number of substrings */
					res += ans;
				}
				
				return res;
			}
		};
Consider: https://leetcode.com/problems/fruit-into-baskets/

	class Solution {
		public:
			int totalFruit(vector<int>& tree) {
				
				int n = tree.size();
				unordered_map<int, int>hm;
				
				int i = 0, j = 0, res = INT_MIN;
				for (i = 0; i < n; i++) {
					hm[tree[i]]++; /* have some hashmap or counter */
					
					/* Loop inside for to reduce the window size based on constraint */
					while (j < n && hm.size() > 2) {
						hm[tree[j]]--;
						if (hm[tree[j]] == 0)
							hm.erase(tree[j]);
						j++;
					}
					/* get the maximum value of the window which satisfies the constraint */
					res = max(res, i-j+1);
				}

				return res == INT_MIN ? (hm.size() > 0 ? n : 0) : res;
				
			}
		};

```
class Solution {
public:
	int findMaxLength(vector<int>& nums) {

		int n = nums.size();
		int mxlen = 0;

		unordered_map<int, int>m;
		int sum  = 0;

		m[0] = -1;
		for (int i = 0; i < n; i++) {
			sum += nums[i] == 1 ? 1 : -1;
			if (m.find(sum) != m.end()) {
				mxlen = max(mxlen, i-m[sum]);
			} else {
				m[sum] = i;
			}
		}

		return mxlen;
	}
};        
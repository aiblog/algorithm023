1.有效的字母异位词
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) return false;
        int n = s.size();
        int arr_1[26]{0};//构建两个字符串的哈希表然后初始化
        int arr_2[26]{0};
        for(int i = 0; i < n;++i){//统计两个字符串中字母分别出现的次数
            ++arr_1[s[i]-'a'];
            ++arr_2[t[i]-'a'];
        }
        for(int i = 0; i < 'z'-'a'+1;++i)
            if(arr_1[i] != arr_2[i])//每个字母进行对比，如有不同则运行完毕
                return false;

        return true;
    }
};
2.两数之和
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int,int> a;
        vector<int> b(2,-1);
        for(int i=0;i<nums.size();i++)
        {
            if(a.count(target-nums[i])>0)
            {
                b[0]=a[target-nums[i]];
                b[1]=i;
                break;
            }
            a[nums[i]]=i;
        }
        return b;
    };
};
3. N叉树的前序遍历
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {

public:
    vector<int> preorder(Node* root) {
        vector<int> result;
        if (root == NULL) return result;
        stack<Node*> st;
        st.push(root);
        while (!st.empty()) {
            Node* node = st.top();
            st.pop();
            result.push_back(node->val);
            for (int i = node->children.size() - 1; i >= 0; i--) {
                if (node->children[i] != NULL) {
                    st.push(node->children[i]);
                }
            }
        }
        return result;
    }
};




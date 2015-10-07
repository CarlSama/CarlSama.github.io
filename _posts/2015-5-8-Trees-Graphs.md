---
layout : post
time : 2015-5-8
title : Trees and Graphs
category : Algorithm
keywrods : Algorithm, CareerCup 
tags : CareerCup 
description : Reading note of < Cracking the code interview >
---

## Tree

### Binary tree is not all binary search tree 

### Traversal algorithm

#### In-order

#### Pre-order

#### Post-order

#### Insert node

#### Balance search tree

#### deletion in search tree

### Check balance 
{% highlight C++ %}
static int maxDepth(TreeNode* root){
	if(root == 0) return 0;
	return max(root->left,root->right)+1;
}
static int minDepth(TreeNode* root){
	if(root == 0) return 0;
	return min(root->left,root->right)+1;
}
static bool isBalanced(TreeNode* root){
	return maxDepth(root) - min(Depth) <= 1;
}
{% endhighlight %}

## Graph

### Traversal algorithm

#### Depth First 

#### Breadth First

### Find first comman ancestor in binary tree

(1)	ask what first means, bottom to top or top to bottom ?
(2)	do each node have parent field ?
(3) can i use additional buffer ?	
(4) does p and q guarenteed to appear in the tree ?

1.	if have parent field, trace up. Without the help of additional buffer, we may need O(2*n) time
2.	Find the first node the p & q locate in the different side of him. Because we traversal the tree each time we need check current node, so some node may be touch many time !
{% highlight C++ %}
bool isAncestor(TreeNode* root,TreeNode* dest) {
	if(root == 0 || dest == 0)	return  false;
	return isAncestor(root->left,dest) || isAncestor(root->right,dest) || root == dest;
}
TreeNode* findCommonAncestor(TreeNode* root,TreeNode* p,TreeNode* q){
	if(isAncestor(root->left,p) || isAncestor(root->left,q))	
		return findCommonAncestor(root->left,p,q);
	else if(isAncestor(root->left,q) || isAncestor(root->right,p))	
		return findCommonAncestor(root->right,p,q);
	else
		return root;
}
{% endhighlight %}
3.	We not only check wether p q is on left/right side, we also record how many node are matched in each side.
{% highlight C++ %}
int cover(TreeNode* root,TreeNode* p,TreeNode* q) {
	int res = 0;
	if(root == 0) return res;
	if(root == q || root === p)  ++res;
	res += cover(root->left,p,q);
	if(res == 2) return 2;//借助于０／１／２的剪枝
	return res += cover(root->right,p,q);
}
TreeNode* findCommonAncestor(TreeNode* root,TreeNode* p,TreeNode* q) {
	if(root==0||p==0||q==0) return 0;
	if(root==p || root==q) return root;

	int leftCover = cover(root->left,p,q);
	if(leftCover == 2) {
		return findCommonAncestor(root->left,p,q);
	}

	int rightCover = cover(root->right,p,q);
	if(rightCover == 2) {
		return findCommonAncestor(root->right,p,q);
	}

	if(leftCover==1 && rightCover ==1) 
		return root;
	else
		return 0;
}
{% endhighlight %}

### Decide if tree2 is sub-tree of tree1

1.	If we can use enough memory, we can record the pre-order and in-order of tree1 and tree2, check wether tree2 is always sub-string of tree1.We can check this using **suffix tree**

2.	Visit certain node in tree1 and each node in tree2, if k is the number of occurences of tree2 root in tree1, the worst time can be characerized as O(n + k*m) 
{% highlight C++ %}
bool matchTree(TreeNode* tree1,TreeNode* tree2) {
	if(tree1==0 && tree2==0)	return true;
	if(tree1==0 || tree2==0)	return false;
	if(tree1->data != tree2->data)	return false;
	return matchTree(tree1->left,tree2->left) && matchTree(tree1->right,tree2->right);
}
bool containsTree(TreeNode* tree1,TreeNode* tree2) {
	if(tree1 == 0)
		return tree2 == 0;
	if(tree2 == 0) 
		return true;
	if(tree1->data == tree2->data){
		if(match(tree1,tree2))//may or may not be result
			return true;
	}
	return containsTree(tree1->left,tree2) || containsTree(tree1->right,tree2);
}
{% endhighlight %}

### Find patch in binary tree which adds up to particular value

{% highlight C++ %}
{% endhighlight %}

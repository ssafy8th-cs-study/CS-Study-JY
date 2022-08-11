# AVL Tree

</br>

## AVL 트리(AVL Tree)

### AVL 트리란
AVL 트리는 이진탐색트리의 단점을 보완하기 위한 트리. <br/>
노드의 추가나 삭제 시 **스스로 균형을 잡는 이진 탐색 트리**라고 할 수 있음. <br/>
트리가 한쪽으로 치우쳐 자라나는 현상을 방지하여 트리 높이의 균형을 유지하는 이진탐색트리.<br/>
균형 잡힌 이진탐색트리를 만들면 N개의 노드를 가진 트리의 높이가 O(logN)이 되어 탐색, 삽입, 삭제 연산의 수행시간이 O(logN)이 보장된다.
![avl_tree](./img/[자료구조]avl_tree_img.png)

<br/>

- 이진탐색트리는 삽입순서에 따라 한쪽으로 노드가 쏠릴 수 있다는 문제점이 있었음.
    ![bst_skewed](./img/[자료구조]bst_skewed.png)
    - 위와 같은 형태의 트리에서 특정값을 찾으려면 O(N)의 시간이 필요.
    - 예로 1을 찾으려면 모든 노드를 탐색해야지 찾을 수 있고 따라서 성능이 매우 나빠지게 됨.
    - 이진탐색트리 연산 실행시간은 이진탐색트리의 높이에 따라 달라지는데 최상의 성능을 얻으려면 트리의 균형을 유지해야 함. (균등 트리일 때는 시간복잡도가 O(logN)이 나오지만 편향 트리일 때는 O(N))
    - 탐색, 삽입, 삭제 연산의 수행시간이 항상 O(logN)이 나오도록 보장하기 위해 AVL 트리를 사용하는 것.

<br/>

- AVL트리는 스스로 균형을 잡으므로 한쪽으로 노드가 쏠리지 않아 시간복잡도가 보장됨.
    ![avl_tree_1](./img/[자료구조]avl_tree_1.jpg)

### 이진탐색트리 vs AVL트리 시간복잡도
|이진탐색트리|평균|최악|
|:---|:---:|:---:|
|삽입|$O(logn)$|$O(n)$|
|삭제|$O(logn)$|$O(n)$|
|탐색|$O(logn)$|$O(n)$|

|AVL트리|평균|최악|
|:---|:---:|:---:|
|삽입|$O(logn)$|$O(logn)$|
|삭제|$O(logn)$|$O(logn)$|
|탐색|$O(logn)$|$O(logn)$|

### 균형도(Balance Factor; BF)
- AVL 트리는 균형도라는 것을 이용하여 삽입과 삭제 시에 트리의 균형을 맞춤.
- 각 노드마다 균형도가 있으며 균형도가 절대값 2 미만이면 **균형트리**, 절대값 2 이상이면 **불균형 트리**임. (AVL 트리는 모든 노드의 BF가 -1, 0, 1 중 하나여야 함. 이를 벗어나면 균형이 깨진 것이고, 이 때 회전이 필요.)
- 불균형 트리의 경우 균형을 맞추기 위한 연산이 이뤄지기 때문에 균형도가 2 이상인 AVL 트리는 존재하지 않음.
- 균형도를 계산하기 위해선 각 노드의 높이부터 계산해야 함.

<br/>

- 높이 구하기 :
    - 높이는 리프노드에서부터 아래에서 위로 계산함
    - **리프 노드의 높이는 항상 0**
    - 부모 노드의 높이는 두 자식 노드의 높이 중 더 큰 값에 1을 더하여 계산됨.
    - **부모 노드의 높이 = Max(왼쪽 자식 노드의 높이, 오른쪽 자식 노드의 높이)+1**
    - **자식이 없는 노드는 null 노드라고 하며 높이를 -1로 가정함**
        ![avl_tree_2](./img/[자료구조]avl_tree_2.jpg)
    
<br/>

- 균형도 구하기 :
    - **부모 노드 균형도 = 왼쪽 자식 노드 높이 - 오른쪽 자식 노드 높이**
    - 균형도 또한 리프노드에서부터 아래에서 위로 계산함
    - **리프 노드의 균형도는 항상 0**
    - **null 노드는 높이와 마찬가지로 균형도가 -1**
        ![avl_tree_3](./img/[자료구조]avl_tree_3.jpg)

<br/>

- 균형도를 보면 좌측으로 쏠려있는지 우측으로 쏠려있는지 알 수 있는 지표가 됨.
    - **균형도가 양수**이면 **좌측으로 쏠려있는 트리**
    - **균형도가 음수**이면 **우측으로 쏠려있는 트리**
        ![avl_tree_4](./img/[자료구조]avl_tree_4.jpg)

### 불균형 문제
- 트리의 삽입 또는 삭제로 인해 트리의 균형도가 무너질 수 있으며, 균형도가 무너진 불균형 트리는 균형도가 양수냐 음수냐에 따라 4가지 문제로 나눌 수 있음.
    1. `LL 문제` : 삽입 또는 삭제로 인해 Left-Left로 서브 트리가 비대해지는 것을 LL문제라고 함.
        - 부모노드의 균형도와 왼쪽 자식노드의 균형도 양수
    2. `RR 문제` : 삽입 또는 삭제로 인해 Right-Right로 서브 트리가 비대해지는 것을 RR문제라고 함.
        - 부모노드의 균형도와 오른쪽 자식노드의 균형도 음수
            ![LL,RR](./img/[자료구조]avl_tree_5.jpg)
    3. `LR 문제` : 삽입 또는 삭제로 인해 Left-Right로 서브 트리가 비대해지는 것을 LR문제라고 함. (좌측이 비대한 문제와 우측이 비대한 문제가 섞인 문제)
        - 부모노드의 균형도 양수 왼쪽 자식노드의 균형도 음수
    4. `RL 문제` : 삽입 또는 삭제로 인해 Right-Left로 서브 트리가 비대해지는 것을 RL문제라고 함. (우측이 비대한 문제와 좌측이 비대한 문제가 섞인 문제)
        - 부모노드의 균형도 음수 오른쪽 자식노드의 균형도 양수
            ![LR, RL](./img/[자료구조]avl_tree_6.jpg)

<br/>

- 데이터 삽입이나 삭제 후에 균형도를 체크.
- 이 4가지 문제를 해결하기 위해 노드를 회전시켜서 균형도를 2 미만으로 맞춰야, 불균형 트리를 균형 트리로 만들 수 있음
    - LL 문제 해결 -> `LL 회전` : LL 문제를 해결하기 위한 회전
        1. 부모 노드를 왼쪽 자식노드의 오른쪽 자식으로 연결
        2. 왼쪽 자식의 오른쪽 자식을 부모 노드의 왼쪽 자식으로 연결
        3. 새로운 부모 노드로 승격
        4. 트리 높이 재계산
        ![LL 회전](./img/[자료구조]avl_tree_7.jpg)
    - RR 문제 해결 ->`RR 회전` : RR 문제를 해결하기 위한 회전
        1. 부모 노드를 오른쪽 자식노드의 왼쪽 자식으로 연결
        2. 오른쪽 자식의 왼쪽 자식을 부모 노드의 오른쪽 자식으로 연결
        3. 새로운 부모 노드로 승격
        4. 트리 높이 재계산
        ![RR 회전](./img/[자료구조]avl_tree_8.jpg)
    - LR 문제 해결 -> `LR 회전` =  `RR 회전` + `LL 회전`
        1. 부모 노드를 오른쪽 자식노드의 왼쪽 자식으로 연결
        2. 오른쪽 자식의 왼쪽 자식을 부모 노드의 오른쪽 자식으로 연결
        3. 새로운 부모 노드로 승격
        4. 트리 높이 재계산
        5. 부모 노드를 왼쪽 자식노드의 오른쪽 자식으로 연결
        6. 왼쪽 자식의 오른쪽 자식을 부모 노드의 왼쪽 자식으로 연결
        7. 새로운 부모 노드로 승격
        8. 트리 높이 재계산
        ![LR 회전](./img/[자료구조]avl_tree_9.jpg)
    - RL 문제 해결 -> `RL 회전` =  `LL 회전` + `RR 회전`
        1. 부모 노드를 왼쪽 자식노드의 오른쪽 자식으로 연결
        2. 왼쪽 자식의 오른쪽 자식을 부모 노드의 왼쪽 자식으로 연결
        3. 새로운 부모 노드로 승격
        4. 트리 높이 재계산
        5. 부모 노드를 오른쪽 자식노드의 왼쪽 자식으로 연결
        6. 오른쪽 자식의 왼쪽 자식을 부모 노드의 오른쪽 자식으로 연결
        7. 새로운 부모 노드로 승격
        8. 트리 높이 재계산
        ![RL 회전](./img/[자료구조]avl_tree_10.jpg)

### AVL 트리의 삽입과 삭제
- 삽입 : 
    - 이진탐색트리의 삽입과 동일하게 새로운 노드를 삽입한다.
    - 새로이 삽입한 노드로부터 루트노드로 거슬러 올라가며 각 노드의 서브트리 높이 차이를 갱신한다. 이때 가장 먼저 불균형이 발생한 노드를 발견하면, 이 노드를 기준으로 새 노드가 어디에 삽입되었는가에 따라 적절한 회전 연산을 수행한다.
<br/><br/>
- 삭제 :
    - 이진탐색트리에서와 동일한 삭제 연산을 수행한다.
    - 삭제된 노드로부터 루트노드 방향으로 거슬로 올라가며 불균형이 발생한 경우 적절한 회전연산을 수행한다.
    - 회전연산 수행 후에 부모노드에서 불균형이 발생할 수도 있고, 이러한 일이 반복되어 루트노드에서 회전연산을 수행해야 하는 경우도 발생한다. 

<br/>

- 자바 구현 코드 :
    ```java
    //import classes and packages  
    import java.util.Scanner;  
    
    // create Node class to design the structure of the AVL Tree Node  
    class Node  
    {      
        int element;  
        int h;  //for height  
        Node leftChild;  
        Node rightChild;  
        
        //default constructor to create null node  
        public Node()  
        {  
            leftChild = null;  
            rightChild = null;  
            element = 0;  
            h = 0;  
        }  
        // parameterized constructor  
        public Node(int element)  
        {  
            leftChild = null;  
            rightChild = null;  
            this.element = element;  
            h = 0;  
        }       
    }  
    
    // create class ConstructAVLTree for constructing AVL Tree  
    class ConstructAVLTree  
    {  
        private Node rootNode;       
    
        //Constructor to set null value to the rootNode  
        public ConstructAVLTree()  
        {  
            rootNode = null;  
        }  
        
        //create removeAll() method to make AVL Tree empty  
        public void removeAll()  
        {  
            rootNode = null;  
        }  
        
        // create checkEmpty() method to check whether the AVL Tree is empty or not  
        public boolean checkEmpty()  
        {  
            if(rootNode == null)  
                return true;  
            else   
                return false;  
        }  
        
        // create insertElement() to insert element to to the AVL Tree  
        public void insertElement(int element)  
        {  
            rootNode = insertElement(element, rootNode);  
        }  
        
        //create getHeight() method to get the height of the AVL Tree  
        private int getHeight(Node node )  
        {  
            return node == null ? -1 : node.h;  
        }  
        
        //create maxNode() method to get the maximum height from left and right node  
        private int getMaxHeight(int leftNodeHeight, int rightNodeHeight)  
        {  
        return leftNodeHeight > rightNodeHeight ? leftNodeHeight : rightNodeHeight;  
        }  
        
        
        //create insertElement() method to insert data in the AVL Tree recursively   
        private Node insertElement(int element, Node node)  
        {  
            //check whether the node is null or not  
            if (node == null)  
                node = new Node(element);  
            //insert a node in case when the given element is lesser than the element of the root node  
            else if (element < node.element)  
            {  
                node.leftChild = insertElement( element, node.leftChild );  
                if( getHeight( node.leftChild ) - getHeight( node.rightChild ) == 2 )  
                    if( element < node.leftChild.element )  
                        node = rotateWithLeftChild( node );  
                    else  
                        node = doubleWithLeftChild( node );  
            }  
            else if( element > node.element )  
            {  
                node.rightChild = insertElement( element, node.rightChild );  
                if( getHeight( node.rightChild ) - getHeight( node.leftChild ) == 2 )  
                    if( element > node.rightChild.element)  
                        node = rotateWithRightChild( node );  
                    else  
                        node = doubleWithRightChild( node );  
            }  
            else  
                ;  // if the element is already present in the tree, we will do nothing   
            node.h = getMaxHeight( getHeight( node.leftChild ), getHeight( node.rightChild ) ) + 1;  
            
            return node;  
            
        }  
        
        // creating rotateWithLeftChild() method to perform rotation of binary tree node with left child        
        private Node rotateWithLeftChild(Node node2)   //LL회전
        {  
            Node node1 = node2.leftChild;  
            node2.leftChild = node1.rightChild;  
            node1.rightChild = node2;  
            node2.h = getMaxHeight( getHeight( node2.leftChild ), getHeight( node2.rightChild ) ) + 1;  
            node1.h = getMaxHeight( getHeight( node1.leftChild ), node2.h ) + 1;  
            return node1;  
        }  
    
        // creating rotateWithRightChild() method to perform rotation of binary tree node with right child        
        private Node rotateWithRightChild(Node node1)  //RR회전
        {  
            Node node2 = node1.rightChild;  
            node1.rightChild = node2.leftChild;  
            node2.leftChild = node1;  
            node1.h = getMaxHeight( getHeight( node1.leftChild ), getHeight( node1.rightChild ) ) + 1;  
            node2.h = getMaxHeight( getHeight( node2.rightChild ), node1.h ) + 1;  
            return node2;  
        }  
    
        //create doubleWithLeftChild() method to perform double rotation of binary tree node. This method first rotate the left child with its right child, and after that, node3 with the new left child  
        private Node doubleWithLeftChild(Node node3)  //LR회전 : RR회전 + LL회전 
        {  
            node3.leftChild = rotateWithRightChild( node3.leftChild );  
            return rotateWithLeftChild( node3 );  
        }  
    
        //create doubleWithRightChild() method to perform double rotation of binary tree node. This method first rotate the right child with its left child and after that node1 with the new right child  
        private Node doubleWithRightChild(Node node1)  //RL회전 : LL회전 + RR회전
        {  
            node1.rightChild = rotateWithLeftChild( node1.rightChild );  
            return rotateWithRightChild( node1 );  
        }      
    
        //create getTotalNumberOfNodes() method to get total number of nodes in the AVL Tree  
        public int getTotalNumberOfNodes()  
        {  
            return getTotalNumberOfNodes(rootNode);  
        }  
        private int getTotalNumberOfNodes(Node head)  
        {  
            if (head == null)  
                return 0;  
            else  
            {  
                int length = 1;  
                length = length + getTotalNumberOfNodes(head.leftChild);  
                length = length + getTotalNumberOfNodes(head.rightChild);  
                return length;  
            }  
        }  
    
        //create searchElement() method to find an element in the AVL Tree  
        public boolean searchElement(int element)  
        {  
            return searchElement(rootNode, element);  
        }  
    
        private boolean searchElement(Node head, int element)  
        {  
            boolean check = false;  
            while ((head != null) && !check)  
            {  
                int headElement = head.element;  
                if (element < headElement)  
                    head = head.leftChild;  
                else if (element > headElement)  
                    head = head.rightChild;  
                else  
                {  
                    check = true;  
                    break;  
                }  
                check = searchElement(head, element);  
            }  
            return check;  
        }  
        // create inorderTraversal() method for traversing AVL Tree in in-order form  
        public void inorderTraversal()  
        {  
            inorderTraversal(rootNode);  
        }  
        private void inorderTraversal(Node head)  
        {  
            if (head != null)  
            {  
                inorderTraversal(head.leftChild);  
                System.out.print(head.element+" ");  
                inorderTraversal(head.rightChild);  
            }  
        }  
    
        // create preorderTraversal() method for traversing AVL Tree in pre-order form  
        public void preorderTraversal()  
        {  
            preorderTraversal(rootNode);  
        }  
        private void preorderTraversal(Node head)  
        {  
            if (head != null)  
            {  
                System.out.print(head.element+" ");  
                preorderTraversal(head.leftChild);               
                preorderTraversal(head.rightChild);  
            }  
        }  
        
        // create postorderTraversal() method for traversing AVL Tree in post-order form  
        public void postorderTraversal()  
        {  
            postorderTraversal(rootNode);  
        }  
        
        private void postorderTraversal(Node head)  
        {  
            if (head != null)  
            {  
                postorderTraversal(head.leftChild);               
                postorderTraversal(head.rightChild);  
                System.out.print(head.element+" ");  
            }  
        }       
    }  
    
    // create AVLTree class to construct AVL Tree  
    public class AVLTreeExample  
    {  
        //main() method starts  
        public static void main(String[] args)  
        {              
            //creating Scanner class object to get input from user  
            Scanner sc = new Scanner(System.in);  
    
            // create object of ConstructAVLTree class object for costructing AVL Tree  
            ConstructAVLTree obj = new ConstructAVLTree();   
    
            char choice;    //initialize a character type variable to choice   
            
            // perform operation of AVL Tree using switch  
            do      
            {  
                System.out.println("\nSelect an operation:\n");  
                System.out.println("1. Insert a node");  
                System.out.println("2. Search a node");  
                System.out.println("3. Get total number of nodes in AVL Tree");  
                System.out.println("4. Is AVL Tree empty?");  
                System.out.println("5. Remove all nodes from AVL Tree");  
                System.out.println("6. Display AVL Tree in Post order");  
                System.out.println("7. Display AVL Tree in Pre order");  
                System.out.println("8. Display AVL Tree in In order");  
    
                //get choice from user  
                int ch = sc.nextInt();              
                switch (ch)  
                {  
                    case 1 :   
                        System.out.println("Please enter an element to insert in AVL Tree");  
                        obj.insertElement( sc.nextInt() );                       
                        break;                            
                    case 2 :   
                        System.out.println("Enter integer element to search");  
                        System.out.println(obj.searchElement( sc.nextInt() ));  
                        break;                                            
                    case 3 :   
                        System.out.println(obj.getTotalNumberOfNodes());  
                        break;       
                    case 4 :   
                        System.out.println(obj.checkEmpty());  
                        break;       
                    case 5 :   
                        obj.removeAll();  
                        System.out.println("\nTree Cleared successfully");  
                        break;  
                    case 6 :   
                        System.out.println("\nDisplay AVL Tree in Post order");  
                        obj.postorderTraversal();  
                        break;  
                    case 7 :   
                        System.out.println("\nDisplay AVL Tree in Pre order");  
                        obj.preorderTraversal();  
                        break;  
                    case 8 :   
                        System.out.println("\nDisplay AVL Tree in In order");  
                        obj.inorderTraversal();  
                        break;  
                    default :   
                        System.out.println("\n ");  
                        break;      
                }  
                System.out.println("\nPress 'y' or 'Y' to continue \n");  
                choice = sc.next().charAt(0);                          
            } while (choice == 'Y'|| choice == 'y');         
        }  
    } 
    출처 : https://www.javatpoint.com/avl-tree-program-in-java
    ```
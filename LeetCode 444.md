

"""

    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        Map<Integer, Set<Integer>> map = new HashMap<Integer, Set<Integer>>();
        Map<Integer, Integer> indegree = new HashMap<Integer, Integer>();
        
        for(int i : org){
            map.put(i, new HashSet<>());
            indegree.put(i, 0);
        }
        int count = 0;
        for(List seq : seqs){
            count += seq.size();
            if(seq.size() > org.length){
                return false;
            }
            if( seq.size() >= 1){
                if(seq.get(0) <= 0 || seq.get(0) > n){
                    return false;
                }
                for(int i = 1; i < org.length; i++){
                    if(seq[i] <= 0 || seq.get(i) > n){
                        return false;
                    }
                    int prereq = seq.get(i-1);
                    int next = seq.get(i);
                    indegree.put(next, indegree.get(i) + 1);
                    if(!map.get(prereq).contains(next)){
                        map.get(prereq).add(next);
                    }
                }
            }
        }
        if(count < n) return false;
        
        Queue<Integer> q = new LinkedList<>();
        for(int i : org){
            if(indegree.get(i) == 0){
                q.add(i);
            }
        }
        if(q.size() > 1){
            return false;
        }
        int total = 0;
        while(q.size() == 1){
            int cur = q.poll();
            if(cur != org[total])   return false;
            total++;
            for(int next : map.get(cur)){
                map.put(next, map.get(next) - 1);
                if(map.get(next) == 0){
                    q.add(next);
                }
            }
        }
        return total == org.length;   
    }


"""

1. 建立一个map [prerequisite：所有follower的集合]
2. 建立一个indegree [number：prerequisite的个数]
3. 一共有 n = org.length这么多个数字，区间为[1, n]
4. 检查seqs中每一个数字是否在这个区间内，并且统计一共有多少个数字(含重复)，检查的同时初始化map和indegree
5. 找出起点，indegree 为0的数字然后加入queue
6. queue中始终只有一个数字。因为同层不可以有多个数字，如果有多个数字则有多种sequence
7. 每次poll出来的那个值等于相对于的org里的值
8. 把element 的followers这个set遍历一遍，把set中每个元素的prereq个数减去1，因为已经满足了一个prerequisite了。当所有prerequisite都满足了的时候就可以加入queue了。
9.  遍历的时候统计有多少层(每层一个所以多少层就是多少个) 最后这个数字应该和org.length相等。

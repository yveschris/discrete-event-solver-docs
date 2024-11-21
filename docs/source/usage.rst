使用方法
=====

.. 基本代码
.. genA函数
.. genR函数
.. genD函数

基本代码
------------

.. code-block:: matlab
   clear
   % clc
   seed = 42;  % select a seed
   rng(seed);

   pkts_type = 'exponential';   % packet size type
   inta_type = 'exponential';   % interarrival time type

   router_num = 2;
   host_num = 0;  % zero means using the default value

   gamma = 0.05;

   edge_spds_r = 1;  % edge rates for routers
   dropprobs_r = 0;

   edge_spds_rh = Inf;
   dropprobs_rh = 0;

   avg_size = 1;  % unit is bit, while the size of payload is byte.
   pld_avg_size = avg_size / 8;

   start_time = 0;
   end_time = 1e3;
   % duplexd = true;

   attmt_assumpt = 'evenly';
   topotype = 'grid';
   [A, Aspds, Adrps, ur, vh, topoinfo] = genA(topotype, edge_spds_r, dropprobs_r, router_num, ...
      host_num, attmt_assumpt, edge_spds_rh, dropprobs_rh);

   router_num = topoinfo.router_num;
   host_num = topoinfo.host_num;

   router_idcs = 1:router_num;
   node_num = router_num + host_num;
   host_idcs = (router_num + 1):node_num;
   proto_stck = {};

   Gh = digraph(A);
   fig_on = true;
   % fig_on = false;
   fig_idx = 0;

   if fig_on
      fig_idx = fig_idx + 1;
      mkrs = ['o', 's'];
      colrs = [1 0 0; 0 1 0];
      figure(fig_idx);
      gtype = [ones(1, router_idcs(end)), repmat(2, 1, host_idcs(end) - router_idcs(end))];
      pgt = plot(Gh, 'Marker', num2cell(mkrs(gtype)), 'NodeColor', colrs(gtype, :), 'Layout', 'force');
      labeledge(pgt, 1:numedges(Gh), 1:numedges(Gh));
   end

   routalgo_assumpt = 'shortestpath'; 
   R = genR(A, node_num, routalgo_assumpt);

   demandtype = 'const';
   [D, demandinfo] = genD(host_num, demandtype);
   D = gamma * D;


genA函数
----------------
generate adjacency matrix function：生成邻接矩阵函数

genR函数
----------------
generate routing matrix function：生成路由矩阵

genD函数
----------------
generate demand matrix：生成需求矩阵


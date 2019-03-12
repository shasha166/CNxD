# CNxD
1) Learning Chow_Liu tree:

    python CLT_class.py -dir  '../dataset/'  -data_name  'nltcs'

2) Learning Mixture of Chow_Liu tree:

    python MIXTURE_CLT.py -dir   '../dataset/'   -data_name   'nltcs'  -ncomp   5  -max_iter   100   -epsilon   1e-7
    
        -ncomp: how many components in the mixture
        -max_iter and -epsilon: the stopping criteria

3) Learning Cutset Network from Data:

    python CNET_class.py  -dir   '../dataset/'   -data_name   'nltcs'  -max_depth  10

4) Learning CNxD:

    python CNXD.py  -dir   '../dataset/'   -data_name   'nltcs'  -alpha  0.5  -function  'root' -min 1 -max 5  -input_module 'nltcs_5'
    
        -alpha: the percentage of using data statistics to learn the cutset network,  0<=alpha<=1
        -function: now only support 'root', 'linear' and square'. Used to adjust alpha by number_of_records_left / total_number_records.

5) Learning Random Cutset Network (RCN): structure is random while parameters are learnt:

    i) learn structure:
    
    python cnr.py  -purpose 'structure' -dir   '../dataset/'   -data_name   'nltcs'  -min_depth 1 -max_depth 5
    
    ii) learn parameters:
    
    python cnr.py  -purpose 'parm' -dir   '../dataset/'   -data_name   'nltcs'  -depth 1  -input_dir '../mt_output/' -input_module 'nltcs_5'
        
        -input_dir and -input_module: the MAP intractable module that used to update the parameters
    
    Note: as the structure is given, we don't need to specify the 'alpha' and 'function'. alpha will be chosen from 0 to 1.0 with 0.1 intervals, function will be chosen from 'root', 'linear' and square'. The output (stored) module is the optimal one with current setting.

6) Learning Bags of Cutset networks:

    python cnet_bag.py  -dir   '../dataset/'   -data_name   'nltcs'  -ncomp   5 -max_depth   5   -sel_opt   0 -depth_option 0

    -sel_opt: could be 0 or 1.
        0 means optimaly select OR node  using MI; 1 means select OR node from 0.5 percent of all varibles
    -depth_option: could be 0,1 or 2 
        0 means all cnets have the same depth (max depth)
        1 means the depth of cnets are randomly choosing from 1 to 6
        2 means the depht of cnets are choosed seqencially from 1 to 6


7) The MAP inference: output are MAP tuples of the test dataset as well as the LL score for the MAP tuples:

        -module_type: 'cnxd', 'cn','rcn','mt' or 'bcnet'
        -epercent: the percentage of evidence variables
        -seq: the index of the the evidence variables records. e.g, if we have 5 sets of evidence, seq will be 0,1,2,3,5
    
    i) For Cutset Networks: cnxd, cn or rcn:
    
    python map_inference.py -module_type 'cnxd' -dir '../dataset/'   -data_name   'nltcs'  -depth 5 -epercent 0.2 -seq 0
    
    ii) For mixture of mt:
    
    python map_inference.py -module_type 'mt' -dir '../dataset/'   -data_name   'nltcs'  -epercent 0.2 -seq 0 -input_module 'nltcs_5'
    
    iii) For bags of cnets:
    
    python map_inference.py -module_type 'bcnet' -dir '../dataset/'   -data_name   'nltcs'  -epercent 0.2 -seq 0


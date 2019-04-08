BEAM-JOINT ATTENTION

This code provides a working implementation of the Beam-Joint attention mechanism as described in the paper (http://aclweb.org/anthology/D18-1065) accepted at EMNLP 2018. For more details on the formulation refer to the paper.

This code builds upon the tensorflow/nmt tutorial repository. We will describe the files which have a significant change from those in the nmt tutorial.

1)model.py : In the _build_decoder method of the BaseModel class, the output_layer is not applied on the outputs.rnn_output since the calls to compute attention in attention_wrapper.py directly return the logits over the output tokens.

2)attention_model.py : The create_attention_mechanism method calls methods from attention_wrapper.py to create attention mechanisms instead of tensorflow/contrib/seq2seq/python/ops/attention_wrapper.py

3)attention_wrapper.py : This file contains a modified AttentionWrapper class whose call function invokes 2 different functions of the AttentionWrapperState class namely _compute_attention and _compute_beam_joint_attention which corresponds to using soft and beam-joint attention respectively. The _compute_beam_joint_attention function contains the code for picking the top-k(by default k=5) most probable memory state probabilities and then mathematically compute the logits for the output tokens.

The additional change to the tensorflow/nmt tutorial repository is that now there is an additional value that the 'attention_architecture' argument can take (attention_architecture=joint) and using this involves using beam-joint attention mechanism.

For implementing different modes Beam-Joint and Full-Joint Attention: Currently this implementation is provided for Beam-Joint with k=5 (If sentences shorter than this are encountered, then all their encoder probabilities are taken into account rather than just the top-k). If you want to change this to Full-Joint Attention, you need to do changes to attention_wrapper.py in the _compute_beam_joint_attention function by removing usage of tf.nn.top_k

How do I cite beam-joint attention?

@inproceedings{ShankarSS18,
  title={Surprisingly Easy Hard-Attention for Sequence to Sequence Learning},
  author={Shiv Shankar and Siddhant Garg and Sunita Sarawagi},
  booktitle={Proceedings of the 2018 conference on Empirical Methods in Natural Language Processing},
  year={2018},
  organization={Association for Computational Linguistics}
}

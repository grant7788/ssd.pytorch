# Need to make some change in pytorch 0.5

## data file:
  cp ssd.pytorch/data/coco_labels.txt ~/data/coco/

## ~/.conda/envs/pytorch/lib/python3.6/site-packages/torch/utils/data/sampler.py

  ### line 51
  
  original:
  return iter(torch.randperm(len(self.data_source)).tolist())
  
  change to:
  cpu = torch.device('cpu')
  return iter(torch.randperm(len(self.data_source), device=cpu).tolist())
  
  refer to: https://github.com/amdegroot/ssd.pytorch/issues/155

## train.py
  
  ### line 165:
  original:
  
  images, targets = next(batch_iterator)
  
  change to:
  
  Try:
      Images,target = next(batch_iterator)
  Except StopIteration: #Start a new iteration
      Batch_iterator = iter(data_loader)
      Images,target = next(batch_iterator)
      
  refer to: https://github.com/amdegroot/ssd.pytorch/issues/174
      
      
  ### line 189, 190, 195, 199
  original: .data[0]
  chage to: .item()
  
## layers/functions/detection.py
  ### line 49:
  original: if scores.dim() == 0:
  change to: if scores.size(0) == 0:
